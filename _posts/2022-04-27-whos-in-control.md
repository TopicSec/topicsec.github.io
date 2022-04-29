---
title: "Who’s In Control? On Security Risks of Disjointed IoT Device Management Channels"
author: Team New Folder
categories: [ IoT ]
tags: [ IoT ]
pin: true
---

## Paper Info
- **Paper Name**: Who’s In Control? On Security Risks of Disjointed IoT Device Management Channels
- **Conference**: CCS '21
- **Author List**: Yan Jia, Bin Yuan, Luyi Xing, Dongfang Zhao, Yifan Zhang, XiaoFeng Wang, Yijing Liu1, Kaimin Zheng, Peyton Crnjak, Yuqing Zhang, Deqing Zou, Hai Jin6
- **Link to Paper**: [here](https://dl.acm.org/doi/10.1145/3460120.3484592)
- **Food**: Chocolate Chip Cookies

# Background
As Internet of Things (IoT) devices continue to grow in popularity, many different frameworks have emerged that manage such devices. These frameworks allow users to utilize various applications to control their IoT devices through local connections such as Bluetooth or through other cloud-based services. Examples of such frameworks include ones provided by manufacturers for their own products, such as the Philips Hue app. In addition, there are third-party solutions that handle devices independent from their manufacturer, such as Apple’s HomeKit or Z-Wave. These frameworks, including all device and cloud-side components, are known as device management channels (DMC), and many IoT devices today tend to support many different DMCs out of the box.

The authors of the paper present research that shows that today’s DMC integration is fundamentally flawed, and leaves various channels on the same device completely disjointed, or inadequately coordinated in their security controls. Since DMCs often have full control over the device, they’re all capable of individually determining whether a certain request should be granted. However, such decisions are uncoordinated, meaning that security policies configured/enforced by one DMC channel could be circumvented via another. The authors call this practice chaotic device management, or Codema, and present work that allows an unauthorized party to leverage one DMC to silently bypass device control implemented through a different DMC. To identify Codema flaws in each device, the authors used a model-based approach to identify dependency relations between the state transitions of two DMCs. A state machine model of a DMC is shown below:

![](/assets/img/2022-04-28-whos-in-control/statediagram.png)

In the paper, a sample of 14 popular IoT devices were analyzed for Codema vulnerabilities, and then user studies were conducted to gauge the viability of a Codema-based attack in the wild. All 14 devices were found to be Codema vulnerable, and were responsibly disclosed to the manufacturers. Given the significant impact of this problem, the authors then present a defense mechanism known as Channel Guard (CGuard), which is an access control framework for cross-DMC security management. CGuard works by maintaining control over each DMC’s accessibility status to ensure that no DMC is left in an unexpected accessibility status, either by accident or by an attacker. To demonstrate this concept, the authors deployed CGuard to a proof-of-concept smart light with its own custom DMC and multiple third-party DMCs. This custom DMC exposes CGuard and allows the device owner to turn on/off specific DMCs, preventing them from being left dangling waiting for an attacker, thus preventing a Codema attack. They evaluated the usability and practicality of CGuard via another user study, finding that their approach is acceptable to users, and established that the performance impact was negligible. The source code for the implementation of this custom DMC with CGuard is available online.

# Discussion

The discussion regarding this paper was relatively short, starting off by acknowledging the fact that these are very simple security flaws, with the reviewers surprised that nobody had found this before. While it was agreed that the paper wasn’t a technical marvel or groundbreaking, the impact of the vulnerability presented has major real-world consequences. This was deemed an interesting point, once again highlighting the fact that research ideas do not have to be complex or dense. One main focal point of the discussion was the impact of Codema vulnerabilities, with the sheer severity of them being on par with remote code execution. This is in part due to the attack itself being stealthy up until the moment when it’s actually executed. The reviewers were also impressed with the writing and framing of the paper, the threat model is very well defined and it is clear that a large amount of effort was put into tying together the technical facts with the real-world problems that Codema generates. This effort is important because one of the large challenges with security is user adoption, and finding the best language to convince users that something like Codema is a real problem is paramount. 

The discussion rounded out by discussing the author’s solution to Codema, CGuard. One aspect missing from the paper that the reviewers were concerned about was manufacturer adoption. The included study in the paper discussed user’s feelings about adopting CGuard, however, CGuard is intended to be included with the device, and wasn’t presented as something an end user could add to an already existing device. Because of this, reviewers were curious if a solution similar to CGuard would end up seeing manufacturer adoption or if another solution would instead pop up, considering the authors stated that the manufacturers had deployed mitigations or were in the process of doing so.




