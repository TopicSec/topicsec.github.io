---
title: "Why Users (Don't) Use Password Managers at a Large Educational Institution"
author:  Team Not Yan 
categories: [Password Manager]
tags: [Password Manager, Privacy]
pin: true
---

## Paper Info:
- **Paper Name**: Why Users (Don't) Use Password Managers at a Large Educational Institution
- **Conference** USENIX '22
- **Author List**: Peter Mayer, Karlsruhe Institute of Technology; Collins W. Munyendo, The George Washington University; Michelle L. Mazurek, University of Maryland, College Park; Adam J. Aviv, The George Washington University
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-mayer.pdf)
- **Food**: Ike's


## Paper Recap:


The paper performs an IRB-approved survey and analyzes the adoption of Password Managers (PMs) and password habits among the members (students, faculty, and staff) of George Washington University (GWU). There were 277 participants in the survey, of which 77% were already using PMs. OS built-in, Browser built-n, and third-party are three broad categories of PMs.

The authors do a thorough analysis and answer four research questions: (a) awareness of PMs among participants familiar, (b) strategies they use and if these include PMs, (c) their password strategies for university accounts, and (d) factors influencing the adoption of PM.

They concluded that 91% of participants are aware of PMs. 94% of PM users were satisfied with PMs. Figure 3 shows the different PMs used by the survey participants. Participants use different strategies. The most commonly used strategy is remembering the passwords 70% of participants do this. Next, 60% of them use Browser built-in passwords. Interestingly 10% of the participants reset their passwords each time!

![PMs](/assets/img/2022-09-21-lack-of-pm-use/pms.jpg) 

Usability and ease of use play a huge role in the decision to opt for PMs. Authors asked participants what their likes and dislikes in PMs based on [1]. Table 6 and Table 7  summaries the results of their likes and dislikes respectively. The Participants would use Browser and OS PMs because of their ease of use. Autofill feature and not remembering passwords also motivate them to go for PMs. Sixty-five participants don't use PMs, of which twenty-four had trust issues with PMs. Eighteen of them do not need PMs, and a few of them lack awareness.

![table6](/assets/img/2022-09-21-lack-of-pm-use/like.jpg) 

![table7](/assets/img/2022-09-21-lack-of-pm-use/dislike.jpg) 

Next, the authors suggest some improvements. Institutions should provide free PMs, arrange info sessions and publicize the PMs. In addition, institutions should invest and host third-party PMs. For the PM developers, the authors suggest having some visual icon embedded in the browser or an easy-to-detect password field and warning the user in case of password reuse.



## Discussion

This survey paper is written very professionally. Previous works were more limited, had fewer participants, and less detailed results, but there were no new great insights into the lack of use of password managers.
 
The novelty of the paper, however, might be summarized as the declaration that despite that many secure and well-designed password managers being available to anyone, the problems of password security are still not resolved due to poor personal habits, no matter what password managers implement. Because users demand convenience far more than security, making security improvements seems to be a difficult task for any actual product to do without risking losing its users.

The paper did provide insights into the issue of password reuse. A good alternative title might have been "Password reuse despite password managers" and the breakdown of user behavior in this area is quite interesting. The fact that 10% of people simply use their email address as their password, resetting their password every single time they sign on was definitely surprising, and the fact that these same people who constantly reset their passwords happen to be among the highest number of people who reuse passwords is also very surprising. Why wouldn't these people try all the passwords they know instead of immediately resetting them? Probably more study into the irrational behaviors people have when dealing with password security and exploring why they do it would have been fascinating.

Another underexplored idea is that password managers are really a shifting of security from "what you know" to "what you have".  What you have is a computer that is signed into a password manager relieving you of the cognitive load of remembering and typing in a memorized password. This becomes a single point of failure if you discover the password to the password manager itself. Even more simply, find a computer that is unattended and signed in, and it becomes trivial to download all of the saved passwords from the password manager. More recent insights into the security of 2FA systems which require some aspect of "what you have" like a smartphone authentication app, biometrics (even if just analyzing typewriting patterns), or location sensitivity, could be used to compensate for this single-point of failure, but the demand for convenience over everything else makes implementing this into a password manager difficult.

The paper itself only had a few areas that could have been improved. Data sets were limited to educational institutions in the United States only, not any foreign or non-educational venues. The summary of this paper was very short and could have used more detail in the future improvements/future work sections. Self-reported subjective data such as asking users to rate their skill level or asking them how secure certain passwords are in comparison to other passwords could have been measured in a more quantitative way.  

References:

[1] Sarah Pearman, Shikun Aerin Zhang, Lujo Bauer, Nicolas Christin, and Lorrie Faith Cranor. Why people (don’t) use password managers effectively. In Proc. SOUPS,
2019.
