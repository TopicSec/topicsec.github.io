---
title: "Why Older Adults (Don't) Use Password Managers"
author: Team Group Lorem Ipsum
categories: [Password Manager]
tags: [Password Manager, Privacy]
pin: true
---

## Paper Info
- **Paper Name**: Why Older Adults (Don't) Use Password Managers
- **Conference**: USENIX Security '21
- **Author List**: Hirak Ray, Flynn Wolf, Ravi Kuber, and Adam J. Aviv
- **Link to Paper**: [here](https://www.usenix.org/conference/usenixsecurity21/presentation/ray)
- **Food**: No food

## Prerequisites
Password managers(PMs) are tools for generating and storing passwords for users. In PMs, passwords are stored using cryptographic tools for security purposes. Only users with a master password (the password for a PM) can extract the encrypted passwords. There are a considerable number of PMs on the market, some are free, others are free with extra paid features, and some are commercial. In some PMs, cloud storage is adopted to assist cross-device and cross-platform password synchronization. 

PMs relieve users from the burden of generating new passwords and remembering them. The ease they provide in turn encourages users to generate passwords with high entropy and different passwords for different websites. For this reason, PMs are deemed highly effective in improving user security. 

Generally speaking, PMs can be divided into two categories: Built-in PM and Separately Installed PM. Built-in PMs are built-in extensions/applications for browsers/operating systems. They store passwords for each website/application. Whenever a password is needed, they will automatically detect and assist users to fill in passwords stored for the website/application. Since they have the support of the platforms they are included with, they are easy to configure and use. Separately installed PMs on the other hand, are standalone applications designed for the purpose of generating secure passwords and storing them securely. Passwords are stored in the application, and require additional steps to set up encrypted storage locally or on a configured cloud service, which requires effort from users.

## Background

The subject of password management deals with one of the most important underrepresented issues of security: how the human factor promotes or inhibits good security practices.  Password managers have clear technical benefits over other techniques of storing and using passwords, but there has been quite a resistance to their adoption. The primary benefits of password managers, secure password storage (cloud or local), complex password generation, transfer and use on multiple platforms have led to their adoption by a younger, technologically savvy generation.  But the older generation in particular has a very different perspective than those held by younger security engineers.

This perspective was examined by breaking the older generation down into three different populations: Those who used no password management software, those who used built in PMs, and those who used separately installed PMs.

## PM Adoption Barriers

A large number of factors were discovered that prevent the older population from adopting password managers. The majority of these issues stemmed from a lack of knowledge on how password managers actually work. This is a false fear that password managers have serious security risks, such as the possibility that your password may be easily hacked by third parties, a local computer crash or a web service disappearing may cause the complete loss of all saved passwords.  Other fears include reluctance to pay for password management, often being unaware that excellent free password managers exist. Overestimation in the security of paper notes, and the worry that a master password is incapable of protecting your other passwords. These worries contribute to the idea that drawbacks of password managers outweigh the benefits.

# ![](/assets/img/2021-09-22-password-manager/browsers.png)

Built in password managers are seen by the older generation of users as being more than sufficient. Users see a clear benefit of features such as the auto fill functionality, but have less concerns about third parties gaining access to your web browser's stored passwords than they do about cloud storage. They do not see any significant value in the benefits gained from switching from built-in password managers to cloud storage. Password managers built into web browsers do not require additional installation or configuration, and the older generation has a reluctance to install additional software.

![](/assets/img/2021-09-22-password-manager/pm.png)

## PM Adoption Benefits

Those in the older generation that were convinced to install separate password managers enjoyed the additional security of the automatic password generation function, and the benefit of being required to only have a single memorizable master password. Advanced features were generally not used, and concerns about the security of cloud storage and synchronization between devices still existed however.

There are many suggested ideas to motivate the older generation to use password managers.  More widespread education on how password managers actually work, endorsements by friends and family, and improvements in the ease of setup would all be major factors in increasing the adoption of password managers by the general public.

## Comparison With Previous Paper

The interview script used in this study was originally developed by Pearman in “Why people (don’t) use password managers effectively.” In that study, an interview script was developed and conducted with different age groups. This paper followed the identical interview script, but focused entirely on older adults.  This highlights the difference between the previously obtained results and current results with older adults.

*Common findings in both age groups*:
- Financial accounts get more priority when security is being considered
- Concerns about single point of failures in non PM users (Example: Could a computer crash or a hack of a website cause me to lose access to all of my accounts at once?)
- Autofill functionality is a useful and desired feature
- Built-in Password Manager users have lack of motivation and incentives to use a separately installed PM
- Password storage feature of PM eliminates the need of remembering all passwords.
- Standalone PMs are understood as more secure than the other alternatives 

*Findings specific to older adults*:
- People are unwilling to pay for PMs as they feel their accounts are not important enough
- Unlike the earlier study which showed high password reuse, older adults tend to make more of their passwords unique
- Use of built-in PM is favored over Standalone due to the perceived difficulty in installing new software and migrating passwords over to a new system
- Recommendations by family members is a very strong motivation for older adults to adopt PMs.
- Many fears and uncertainties concerning cloud password storage and synchronization because of misconceptions about security over the Internet
- When using standalone PMs, the selected master password was more often created from a familiar phrase rather than be entirely random

## Discussion

The original paper by Pearman had many limitations and issues that were not addressed in this particular paper. There was still a small total sample size, in this paper of only 26 individuals divided up into three different groups. There was nothing done to account for social desirability bias (tendency of survey respondents to answer questions in a manner that will be viewed favorably by others) and the interviewers were very subjective, interpreting certain statements.  Quite a lot of repetitive language is used in this paper.  This is an issue often in papers that perform  qualitative analysis.

An interesting aspect was the discussion of the contrast between built in PMs and web-based PMs.  Older adults expressed trust issues for both of them, and consider many aspects of PMs in general as being a possible single point of failure.  

Another question not examined by this paper is the issue of how objectively strong certain passwords actually are (as determined by how long it would take a hacker to actually crack them) and evaluating what people think are weak and strong passwords.  For example, what password is more secure, in terms of higher entropy?  8 completely random characters, or a password with 26 characters made up of english words such as “correcthorsebatterystaple.” With a large enough dictionary of English words to throw together, a password of length 26 might be far easier to break than the randomized 8 character password.
