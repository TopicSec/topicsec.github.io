---
title: Security Mistakes Developers Make
author: Team Fable
date: 2020-09-09
categories: []
tags: [qualitative, mistakes]
pin: true
---

## Abstract
This paper aims to study and categorize the various types of mistakes developers make regarding security.
In order to do this, they leveraged a Build It, Break It, Fix It competition to obtain data on the introduction of errors and their subsequent fixes.
Vulnerabilities were categorized according to how they were introduced, level of control they allowed, and how easily they could be exploited.
Once the raw data was categorized, certain trends emerged: outright mistakes where the developer didn't implement what they meant to were relatively uncommon, while the most common type of vulnerability was that introduced through misconceptions of security concepts (API misuse, etc.).

# backround & data

based on a secure programming competition

- 3 phases, build/break/fix

 ## data

### project selection



### data gathered

- secure log
- secure communication
- multiuser database
- project characteristics

# research question

vuln types: conceptual mistakes

how much control gain

require to exploit

# methodology question

- representative of real world?

In Favor and Against



## Error Categorization
They broke observed errors down into several types: no implementation, misunderstandings, and outright mistakes.

### No Implementation
No implementation errors were the simple case where a team did not even attempt to provide a required security feature, such as an implementation of secure log with no integrity checking.
Within the umbrella of no implementation, they sub-divided further to consider *all intuitive*, *some intuitive*, and *unintuitive* separately.
The first two categories comprise those missing features that were at least partially either mentioned in the specification or considered obvious from the requirements.
The last category, unintuitive, comprised errors where the required security feature was not explictly stated or intuitive, such as the need to use MACs to ensure integrity.
Examples of *all intuitive* errors were not using encryption in the secure log and secure communication tasks, as well as not attempting to enforce the access control aspect of the multiuser-database task.
Some teams only partially implemented these access control requirements which was coded as *some intuitive*.
The *unintuitive* errors were more varied. Teams failed to use MACs to ensure integrity in the secure log and secure communication tasks.
There were side channels leaking data in the secure communication and multiuser-database tasks.
Replay attacks could be used against some secure communication implementations.
Finally, user rights were not properly checked in the case of delegation in the multiuser-database task.

### Misunderstandings
A vulnerability was categorized as a misunderstanding when a team attempted to implement security requirements, but made mistakes in doing so.
These were further sub-divided into *bad choices* and *conceptual errors*.

Bad choices represent inherently insecure choices, of which five distinct instances were observed.
Of note among these was the use of bespoke encryption; one team chunked cleartext by the key length and XORed these chunks with the key, trivially allowing the key to be recovered from two blocks of the ciphertext.
Another common issue was using functions that could compromise memory safety, such as using `strcpy`.
The authors chose to class this is a bad choice rather than an outright mistake due the the existence of `strlcpy`.

Conceptual errors represent the misuse of not-inherently-insecure mechanisms.
For example, crytographic algorithms often require random values as initialization and may harm security if not actually random.
Hardcoding passwords was also classified as a conceptual error.
Other issues categorized as conceptual errors included correct usage of security features while failing to protect everything needed to provide security.
For instance, one team ensured integrity of individual log entries but not the log as a whole, ostensibly preventing modification of individual entries but allowing arbitrary deletion/duplication/reordering.
Another example of this issue was the disabling of built-in protections; one team disabled automatic MAC usage in their SQL library.

### Mistakes

The final category of error is outright developer mistakes.
Examples of this type of error include improper error handling leading to the application hanging or crashing.
Other examples include control flow logic errors; one team failed to properly store data they later checked the presence of, leading to their check always returning the same resut.

## quantify exploitable

discovery difficulties

- require execution

- require source code

- require insights on key algorithm

exploit difficulties





# results

## types of errors

that big table in paper

## risk factors of errors prevalence

- skip unintuitive

- more errors in popular language

- practices (code review, testcases) work

- no differences between professional and student
  - discussion of measuring sec profession 35 min
  - practical train/experience?
    - errors are all in course slides
    - they have 8 9 years dev experience
    - CTF?
    - team size/structure
    - so how to train and measure?



## risk factors of attacker control,

report by teams:

no implement: easy to find

misunderstand: hard

mistake: easy to find and exploitable



# recommendation

- match CWE/OWSAP

- hire
- better API design
- better API docs
- better education
- better automatic tools







# discussion:

in slides:

- how competition can measure this field more accurately

- pro vs student

- better static analysis tools





in video:

- dynamic/semantic ways:

hardcoded aes iv, just one type



- no tools on design/conceptual level

GPT3? (hardcoded rules?) a ccs nlp paper



- that unapproachable disassembly paper problem

follow up work
