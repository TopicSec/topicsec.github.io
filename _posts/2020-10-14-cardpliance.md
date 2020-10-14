---
title: Cardpliance
author: Team Olympic
date: 2020-10-13
categories: [software, verification]
tags: [android, pci]
pin: true
---

## Paper Info
- **Paper Name**: Cardpliance: PCI DSS Compliance of Android Applications
- **Conference**: USENIX Security '20
- **Author List**: Samin Yaseer Mahmud, Akhil Acharya, Benjamin Andow, William Enck, Bradley Reaves
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec20-mahmud.pdf)
- **Food**: Pairs well with cupcakes, preferably lemon meringue or s'mores.

## What is PCI DSS

> Widespread adoption of online financial transactions in early 2000s led to widespread payment fraud. As a result, in 2004 PCI (Payment Card Industry) released the Data Security Standard aiming to secure credit card payments. PCI DSS distinguishes between two kinds of data impacting software processing - Cardholder Data (CHD) and Sensitive Account Data (SAD).
> Not all PCI requirements are applicable to mobile applications. Those relevant are:
1. Limit CHD storage
2. Restrict SAD storage
3. Mask PAN when displaying
4. Encrypt CHD when storing
5. User secure communication
6. Secure transmission of PAN to external apps

## Cardpliance

> Are Android applications handling credit card data properly?
Identifying vulnerabilities related to credit card information in mobile applications comes with challenges. No clearly-defined API for identifying and linking inputs, context-sensitive nature and imprecisede definition of DSS requirements make automatic identification non-trivial.
> Cardpliance is a tool, that performs compliance checks on Android applications. How does it work?


### Amandroid
![](/assets/img/2020-10-14-cardpliance/uiref.png)

Cardpliance is built on top of open-source Android application analysis tool, Amandroid [1]. Amandroid was chosen due to its actively maintained state, extensible design, and convenient graphs outputs. It is primarily focused on data flow and performs flow- and context sensitive static program analysis on .apk files. Amandroid uses DDG (Data Dependence Graph) to perform taint analysis.  It marks given sources and sinks in the DDG and computes the set of all paths between them. The list of paths from sources to sinks is stored in a Taint Analysis Result (TAR) structure. User can define sources and sinks via text strings of method signatures in a configuration file.

### Resolving input semantics
![](/assets/img/2020-10-14-cardpliance/dataflow.png)

Applications access user entered text via the `TextView.getText()` method. To acquire a TextView object, the application calls `Activity.findViewById(R.id.widget_name)`, where `R.id.widget_name` is a unique integer managed by the
application’s resource R class. Therefore, Cardpliance uses `Activity.findViewById(int)` as a taint source. The analysis will taint the returned `TextView` and the subsequent string from `TextView.getText()`. Furthermore, since the DDG contains points-to information, the PCI DSS tests can use Amandroid’s `ExplicitValueFinder.findExplicitLiteralForArgs()` method to determine the integer value passed to the taint source. It then uses the resource IDs of credit card information widgets identified by UiRef to determine the types of information flowing to each sink.
However, applications frequently call `Activity.findViewById()` to assess many different UI widgets. Therefore, simply defining it as a taint source will cause Amandroid’s taint analysis to needlessly compute taint paths for many irrelevant sources. To address this problem, Cardpliance implements a custom source and sink manager that refines the taint sources to just those `Activity.findViewById(int)` instructions that are passed an integer in a list precomputed by UiRef. This process involves using the `PTAResult` hash map while marking taint sources. This significantly reduces application analysis time. Additionally, since one of Cardpliance’s tests uses `View.setText()` as a taint sink, similar optimization in the custom source and sink manager is performed. In this case, the call to `Activity.findViewById(int)` is identified by backtracking in the DDG to the definition site of the `View` object. The integer resource ID is resolved similarly. If the ID is in a predefined list (defined via a heuristic for the test), the call to `View.setText()` is defined as a taint sink. Finally, Amandroid’s control flow analysis was patched to properly track the use of `View` objects obtained in `OnClickListener` callbacks. It occured that many applications declare the `OnClickListener` of a `View` as an anonymous inner class. In such cases, Amandroid did not capture the data flow initiated by the button click. This issue was fixed by adding a dummy edge from the point where the `OnClickListener` was registered to the entry point of the corresponding `OnClickListener.onClick()` method.

### PCI DSS Tests
![](/assets/img/2020-10-14-cardpliance/pci_methods.png)

Each test is defined with respect to instructions invoking three sets of methods: source methods (S), sink methods (K), and required methods (R). S and K are traditional sources and sinks for taint analysis. R places requirements on the data flow path. Informally, R defines a set of methods that should be called on the data flow path (e.g., a string manipulation method that could mask characters). If no methods from R exist on the path, then a potential violation is raised.

Six PCI DSS tests with respect to S, K, and R are described:

Test T1 (Storing CHD): Requirement 1 states that storage of cardholder data (CHD) should be limited, and if it is stored, there should be a mechanism to delete if after a period of time. Determining all of the ways in which
persistent data can be deleted is not practical to detect using static program analysis. Test T1 takes a security-conservative approach and identifies whenever CHD is written to persistent storage and is more of a warning than a violation of PCI DSS.

Test T2 (Storing SAD): Requirement 2 states that sensitive account data (SAD) should never be written to persistent storage, including logs. Test T2 only needs to consider `Activity.findViewById(int)` sources that are passed resource IDs corresponding to CVC-related fields. Unlike Test T1, the existence of a data flow path directly represents a PCI DSS violation.

Test T3 (Masking Credit Card Number): Requirement 3 states that the only time the application should display the full credit card number (PAN) is when the user is entering it in the text field. All other times the credit card
number is displayed, it should be masked, showing at most the first six and last four digits of the number. A set of PAN masking methods (PMM), representing different ways of masking the credit card number was defined.

Test T4 (Storing Non-Obfuscated Credit Card Number): Requirement 4 states that the credit card number (PAN) should always be protected if it is stored by the mobile application. The PCI DSS standard has some flexibility in
how the number is protected, but it offers suggestions including one-way hash functions and cryptography. In contrast to Test T1, which captures all cases of CC number written to persistent storage Test T4 only raises an alarm when there is not an obfuscation method on the data flow path. Therefore, Test T4 is designed to detect violations.

Test T5 (Insecure Transmission): Requirement 5 states that mobile applications should always use TLS/SSL when transmitting cardholder data. Two ways the application can fail to properly use TLS/SSL are (1) send data via HTTP URLs, (2) invalidate certificate checks when sending data via HTTPS URLs.

Test T6 (Sharing Non-Obfuscated Credit Card Number): Requirement 6 states that credit card numbers should be protected if they are shared outside of the application. Both SMS APIs and Android’s inter-component communication (ICC) mechanism that allows execution to span applications were considered. Similar to Test T4, this test determines if all paths from sources and sinks include a call to an obfuscation method.

### Compliance study
The goal is to gauge the impact of PCI DSS non-compliance on real-world users. Popular applications from Google Play were analyzed for potential mishandling of payment information. Cardpliance uses static analysis which is subject to limitations such as over-approximation of app behaviours resulting in false positives. Manual validation of data flows that flagged by Cardpliance as potential violations was performed. In the study true positive denotes that the application contains a PCI DSS violation while a false positive denotes that none of the data flows flagged by static analysis were valid due to errors in the underlying tooling.

### Dataset
- top 500 free applications across Google Play's 35 application categories in May 2019 -> Initial dataset of 17,500 applications
- keyword search on resource files for terms describing payment card numbers -> list of terms from the synonym list in UiRef for "credit card number"
- flagged 1,868 applications as potentially requesting credit card information, which reduced the dataset by 89.3% (15,632/17,500)
- UiRef used to determine when an application takes credit card numbers as input
- 807 applications failed during reassembly due to errors in ApkTool [3]. UiRef failed to extract layouts from an additional 110 applications
- 442 applications containing input widgets that request credit card numbers identified by UiRef
- run 15 applications in parallel, set 60-minute timeout per application component
- 80.99% (358/442) of the triaged application dataset successfully analyzed
- of the 19.01% (84/442) applications that failed analysis:
	- 3.84% (17/442) applications contained components that exceeded the timeout
	- 15.15% (67/442) applications could not run due to errors in the underlying static analysis framework Amandroid

### Findings
1. At least 2.5% of popular free Android applications on Google Play directly request payment information.
2. Cardpliance can analyze an application with a mean and median runtime of 334 minutes and 179 minutes, respectively
#### Compliance
3. Around 98.32% of the 358 applications pass Cardpliance’s PCI DSS compliance tests
4. Applications are correctly using HTTPS instead of HTTP to transmit payment information
5. Applications are correctly performing hostname and certificate verification when sending payment information over SSL connections
6. Applications are not insecurely sharing payment information via SMS or with other applications via ICC channels
#### Non-compliance
7. Applications totaling over 1.5 million downloads are not complying with PCI DSS regulations
8. Applications are storing credit card numbers without hashing or encrypting the data
9. Applications are persisting card verification codes (CVCs)
10. Applications are not masking credit card numbers when displaying them in the user interface

### Case studies
![](/assets/img/2020-10-14-cardpliance/pretzel.png)

Two examples of how applications are violating PCI DSS based on Cardpliance analysis

Case Study 1: A credit card reader application is mishandling hundreds-of-thousands of customer’s credit card numbers
	Android app Credit Card Reader (com.ics.creditcardreader) with 500k+ downloads functions similar to POS and accepts physical payments.  The app persistently stores CC numbers without hashing or encryption.
	Potentially hundreds-of-thousands of credit card numbers exposed.

Case Study 2: An application for placing online orders at a restaurant franchise is persisting credit card numbers in plaintext along with CVCs
	A franchise restaurant Ben’s Soft Pretzels has an application on Google Play (com.rt7mobilereward.app.benspretzel) with 10k+ downloads.
	Application allows users to place online orders from the restaurant and accepts credit card payments via the application. Then it stores credit card numbers without hashing or encrypting, stores CVCs, and not masks credit card numbers when displaying.

### Disclosure of Findings
Cardpliance identified 15 PCI DSS violations in 6 applications from Google Play. All developers were notified through email. As of 75 days after disclosure only one responded.

### Threats to validity

- False negatives
  - Initial filter is lightweight and keyword based and also English only
  - T3 is also keyword based, excluding webviews and icons
  - UiRef doesn't handle dynamically generated UIs, which explains the 50% discard
  - Amandroid is fallible
  - No PANs were detected over SMS or implicit intent
  - SSL validation was only checking for certificate validation
  - Assumed only internal explicit intents
- False positives
  - These are much more acceptable - manual filtering is possible
  - findBiewByID(context, id) was an issue because it taints the global context
  - Masking heuristic isn't the best

## Best practices

The following recommendations might be useful for developers building Android apps that use credit card information:

- Delegate responsibility to established third-party processors (Stripe, Square, PayPal)
- Do not write SAD to persistent storage or log files
- Do not write CC number to persistant storage or log files
- Encrypt CC numbers with secure randomly generated keys before storing locally
- Always use secure connection when transmitting over the network
- Never modify the SSLSocketFactory or TrustManager within application code
- Always mask CC number before displaying it
- Only use explicitly-addressed Intent messages when sharing payment information

## Discussion / Criticism

> Numbers do not line up with other studies of PCI-DSS compliance - generally it is found that things are noncompliant at a much higher rate than claimed
> Claims of SSL usage do not line up with related work
> Dataset (top 100 apps) misses the kinds of apps which are most likely to be noncompliant
> Strings filtering was too strict
> Analysis failed on half of the strings-filtered apps
> No indication of why the tool takes so long to run

## References
[[1]](http://pag.arguslab.org/argus-saf) http://pag.arguslab.org/argus-saf

[[2]](https://ibotpeaches.github.io/Apktool/) https://ibotpeaches.github.io/Apktool/
