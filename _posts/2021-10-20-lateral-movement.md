---
title: "Hopper - Modeling and Detecting Lateral Movement"
author: Team bi0s
categories: [defense]
tags: [lateral movement, defense]
pin: true
---

## Paper Info
- **Paper Name**: Hopper - Modeling and Detecting Lateral Movement
- **Conference**: USENIX Security '21
- **Author List**: Grant Ho, Mayank Dhiman, Devdatta Akhawe, Vern Paxson,Stefan Savage, Geoffrey M. Voelker, David Wagner
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec21-ho.pdf)
- **Food**: No food

## Prerequisites
In enterprise networks, attackers try to access more machines to retrieve valuable information. This attempt to jump from machine to machine inside of an internal network is known as lateral movement.  Existing tools to detect such attacks have scalability issues and they generate high levels of false positive alarms.

Hopper works as a type of intrusion detection system by attempting to detect lateral movement that is suspicious and could be a potential attack, while ignoring the vast amount of normal network activity which may behave in similar fashion. Common log data is used by Hopper to detect lateral movements in the network. A graph is built from user movements obtained from log data and suspicious paths are identified using key properties.

To mitigate lateral movements, three general strategies are used.
Using improved security policies
Detecting lateral movement
Analyzing existing attacks and remediate

This work focuses on detecting lateral movement while the other two strategies implemented independently complement Hopper. The authors do not look at firewall intrusion, but assume that at least one machine inside the network has been compromised and that the attacker is trying to make movement within the network. 

![](/assets/img/2021-10-20-lateral-movement/what_is.jpeg)

## Summary

![](/assets/img/2021-10-20-lateral-movement/summary.jpeg)

## Problem
Security inside of a corporate network is only as strong as the security of the weakest employee or workstation. An employee running something from a phishing email or the web can completely bypass the best corporate firewalls.   Security of the entire network should not be compromised if the security of a single machine is compromised.  

Attackers can start with a compromised machine which may have low security and hack higher privileges on the machine. They can then jump to another machine with either greater access to valuable data or greater access to the network.

![](/assets/img/2021-10-20-lateral-movement/lateral_movement.jpeg)

The issue could also be framed in a way of solving the problem of analyzing detailed security logs. If you already have logged a considerable number of login events, it should be possible to assume that suspicious behavior between a compromised machine and a valuable machine should be something that could be detected, solely by analyzing these login or remote command execution events.

Current intrusion detection software already performs similar detection of suspicious in-network activity utilizing either fixed rules or some type of machine learning,  but does so with a high false positive rate.  An ideal solution to this problem should both exhibit high accurate detection of all different types of attacks , while maintaining a far lower false-positive rate. This allows it to scale to very large enterprise networks, such as the internal network of Dropbox that was used to develop this software.  There may be additional issues with the scalabil


## Solution

To detect lateral movement, the first step is to collect successful logins as the dataset. However, the authors noticed that not all successful logins are meaningful data. Some login events are created by operating systems or automation. These automated login events do not help in detecting lateral movement caused by human attackers. The authors cleaned the dataset by detecting and removing logins created by operating systems and low-risk automated logins. After the data cleaning, they successfully reduced the number of login events from 784,459,506 to 3,527,844.

By analyzing the meaningful dataset, the authors created a system that maps the login events as a graph, in which nodes are machines and edges are logins. It detects suspicious login paths made by a single actor that (1) uses another credential other than his/her own and (2) the path accesses machines that the actor do not have access to. These detection rules are based on the intuition that attackers need to use elevated credentials (property 1) to access machines containing sensitive information that their initial victim does not have (property 2).

Hopper consists of two components: the Causality Engine and the Alert Generator. The Causality Engine aims to analyze individual login events and identify the login paths caused by the same actor. However, login events are stateless, it is hard to reconstruct the login path just simply using the individual login events. For example, suppose both person A and B login into server X and a login happens from server X to server Y, there is no way to tell whether A or B performed the login action by monitoring the login events. The authors use different tags to distinguish the paths. The unclear login paths will be marked as UNCLEAR for later analysis. The Alert Generator will receive paths analyzed by Causality Engine and evaluate whether an unusual lateral movement happens. It first filters out commonly seen paths, such as login path using the same credential, switching to service account, using admin account on behalf of some other users, etc.  Hopper then applies two predefined rules on the left paths to identify suspicious login paths. It generates alerts if (1) there is a clear credential switch and the actor never accesses one of the machines in the login path in the past 30 days, and (2) the UNCLEAR login path ranks high in terms of its “suspiciousness” score which is calculated by features characterizing the rareness of the path. Since there can be many UNCLEAR login paths in a day, Hopper only generates alerts for several most suspicious paths each day. The number is adjustable.

## Evaluation

Hopper runs on Python 2.7 on a Linux server.  Using the Dropbox enterprise network login data, 2.5 GB of RAM were used. With the 16 core processor, feature extraction took roughly 83 minutes of cumulative CPU time, and the path generation algorithm took a CPU time of 35 minutes.

To compare Hopper against the current state-of-the-art solution, such as Sal, the authors worked with a 15 month data set of login events captured from Dropbox's internal network.

A diverse collection of 326 simulated attacks from 50 footholds and one actual attack were added to the data set to give both software programs something to detect. The attack simulations were a combination of aggressive spreading attacks logging into all available machines, exploratory attacks cautiously stopping when a single login failed, and targeted attacks that try to access a single high-value target in the shortest path possible.  Stealth techniques of using only recently used account credentials and pathways previously used by legitimate user connections were also used.

![](/assets/img/2021-10-20-lateral-movement/attack_simulations.jpeg)

Dropbox's Red Team conducted an actual attack on their own network using the principles of an unclear pathway, finding administrator credentials that were recently used, and then utilizing them again.  Hopper was able to detect 94.5% of the attacks (309 successful, 18 failed) including the actual attack performed by Dropbox's Red Team.  The red team attack was flagged as the most suspicious pathway detected on that day.  

Half of the failures were due to errors in the log data, and with a higher sensitivity of only 11 alerts per day, all of the attack failures due to stealth could be detected.  IT services performing imaging and provisioning or legitimate use of special service accounts created most of the remaining false positives.

This measurement of accuracy is only significant in terms of how sensitive the program is set. Higher sensitivity levels are able to detect a greater percentage of attacks, but also will increase the number of false positive alerts sent out by the program. Lower sensitivity results in lower accuracy, but also greatly reduces false positive alerts as well.  In achieving a 94.5% detection rate, Hopper was able to be set to a sensitivity level where only 9 false positives were sent per day, 8 times fewer false positives than the current state-of-the-art solution.

![](/assets/img/2021-10-20-lateral-movement/evaluation.jpeg)

## Limitations

Hopper is only a type of intrusion detection and does not prevent any type of attack. It requires certain security policies, such as a 24 hour timeout window, be already implemented. Hopper also does not perform any forensics on security attacks.

Hopper also is only as accurate as the logging information provided to it. Incomplete or inaccurate logs could distort the results. Other problems include only working within the 24 hour session time window. If this time window was changed by malware for example, Hopper could not detect connections.  The same concern applies to piggybacking on legitimate login pathways.

Since Hopper was developed for Dropbox's enterprise network with a very flat topology, there are concerns that different topologies and different time thresholds could introduce path explosions and greatly impact Hopper's performance and accuracy.

## Discussion

This paper is reliant on confidential corporate data of Dropbox to generate results. This makes this paper non-reproducible without having access to this data. This leads to the concerns that flaws could possibly exist that will not be easily found or that other hidden generalizability problems could exist.

This solution may simply not work on other enterprise networks.  The authors worked in tight collaboration with Dropbox and this solution worked well with their network, but enterprise networks are complicated systems with sometimes dozens of physical locations, legacy software and connections, and multiple hierarchical layers and sections.  The simple and flat Dropbox corporate network is probably the exception rather than the rule, and multiple hops through an enterprise network could easily create path explosions and make this tool unusable.

On the other hand, scientific papers sometimes are able to promote clear scientific truths that are not applicable to actual real-world problems. This paper does have the advantage of addressing a real-world problem that doesn't currently have a good solution and is demonstrably proficient at solving it. The authors have written a software program that can actually be deployed immediately and is usable in its current state to detect attacks.

The evaluation was detailed and complete, but another concern is that all of the attacks were theoretically injected into the data set.  Only a single actual attack was performed. More than just 1 actual penetration test conducted under various conditions would do a lot to improve the confidence in how comprehensive and accurate the evaluation is.

The focus of this paper is definitely on a narrow topic, the subject of lateral movement, and their solution to this problem is very elegant. They  don't attempt to solve some of the underlying assumptions in their product. For example, adding delays in hops of greater than 24 hours, assuming the attacker could override the session limitations on each machine they connected to, would make them invisible to Hopper.

Another advantage that Hopper has is that it is useful in detecting both the scouting activity of hackers, and in detecting actual attacks. It also is software that is run separately on log files, and not something that hackers could easily detect such as software that was installed on every PC in a network.  Additional penetration tests to a network with Hopper implemented would be one way to develop and refine these ideas further.

