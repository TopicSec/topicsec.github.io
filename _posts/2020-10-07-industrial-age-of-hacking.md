---
title: The Industrial Age of Hacking
author: Team Fable
date: 2020-10-07
categories: []
tags: [bug-finding,]
pin: true
---

## Paper Info
- **Paper Name**: The Industrial Age of Hacking
- **Conference**: USENIX Security '20
- **Author List**: Tim Nosco, Jared Ziegler, Zechariah Clark, Davy Marrero, Todd Finkler, Andrew Barbarello, W. Michael Petullo
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec20-nosco.pdf)
- **Food**: Pairs well with cupcakes, preferably lemon meringue or s'mores.

# Abstract
At the core of this paper is the observation that most bug- and vulnerability-finding is done without specific indication that these are likely to exist.
The paper terms this the depth-first approach and proposes a new method of searching for vulnerabilities: breadth-first.
Their proposed breadth-first process revolves around the idea of investing minimal human effort early on, just enough to enable automated analysis, and thereby increasing the number of targets that can be investigated.
The aim of the process is threefold: the discovery of bugs, the efficient use and coaching of the people involved, and a quantifiable process with incremental progress.
The authors claim that this breadth-first search strategy increases the overall effectiveness of teams.
Other work has studied these same processes, particularly Votipka, et al., whose vulnerability discovery process is built upon in this paper.

# Vulnerability Discovery
The vulnerability discovery process described primarily adds a *targeting* phase to Votipka, et al.'s process, and this augmented process shown here is described in the following sections.

![](/assets/img/2020-10-07-industrial-age-of-hacking/process.png)

In this paper, bug finders are broken down into three categories comprising the *hackers*: apprentices, journeymen, and masters.
An **apprentice** is characterized by a reliance on automated analysis tools, such as fuzzers.
**Journeymen** are differentiated from apprentices through their ability to modify programs for easier analysis, whether through source code or binary patching.
**Masters** are in turn differentiated by the ability to alter or create analysis tools.
Non-hacker team members include leaders, whose responsibility it is to select targets based on the results of hackers, as well as analysts and support personnel.

## Targeting
As mentioned before, it is the addition of targeting to the Votipka process that represents the largest divergence.
The goal of targeting is to break systems down into targets which may be individually analyzed later on.
Examples given in the paper include a web browser breaking down into distinct targets: networking libraries, the javascript engine, the renderer, etc.
In the described process, the targeting phase for a certain system considers existing analysis work, access to source code and bug trackers, and the impact of potential bugs, among other pragmatic factors.
The overall model for selecting targets is based on the expected profit of analysis increasing with the likelihood of bugs and the value of such a vulnerability and falling with the expected time and required skill level.

## Information Gathering
This is the first step taken by hackers and analysts, with an aim of collecting information needed for later decisions to be made.
Information such as the development process of the target, known previous bugs, and any prior analysis are relevant at this stage.

## Program Understanding
In this phase, hackers gain knowledge of the design and use of the target.
Of particular interest are how the target is commonly deployed, configuration options for more advanced uses, and the overall design of the software itself.

## Attack Surface Analysis
AT a high level, this phase revolves around coming up with ways to supply inputs to various aspects of the target program.
This is the first stage of the process that is highly differentiated by hacker skill level.
Prior work in this area has often pointed out that master-level skillsets are often required to begin work; the authors note finding several counterexamples where apprentices and journeymen were able to discover vulnerabilities without requiring the assistance of a master.

### Apprentices
Apprentices apply standard automated tools as long as they are useful, and bail out at the first sign of trouble.
Their work up to that point is documented, and the apprentice moves on to a new target.

### Journeymen
Journeyman utilize the documentation from prior work by apprentices to skip ahead and utilize their skillset to work around the issues encountered by the apprentices.

### Masters
Masters likewise utilize documentation from prior apprentice and journeyman work to skip directly the aspects requiring their skills, such as those requiring changes to existing analysis software or new forms of analysis entirely.

## Automated Exploration
Since the previous phase involves learning to manipulate program inputs, this phase is free to leverage that knowledge and any fuzzing harnesses produced.
Predictably, fuzzing is the most common manifestation of this stage.

## Vulnerability Recognition
Bugs that are discovered in the previous stage must now be analyzed to determine if they represent a vulnerability.
Certain bugs may lead to more obvious vulnerabilities less-skilled hackers can directly exploit, or they may require the target being handed off to a more skilled hacker.

## Reporting
In this phase, the hacker who actually exploits a vulnerability prepares a report with a goal of enabling the developers to fix the bug.

# Depth-first
In a depth-first targeting strategy, targets are selected based on the predicted impact of discovering a bug.
Targets which are selected are audited by the team entirely, and each target is focused on for a considerable period of time.
The authors note the prior effectiveness of this approach when dealing with large projects requiring a period of familiarization, specifically referring to bugs found in the Safari browser by Google's Project Zero.

The primary pitfall of this strategy is stated to be inefficiency at utilizing individuals of varying skill; anyone might find a bug that an apprentice could exploit, but after a certain point, these apprentice-level issues will be exhausted.
In this case, apprentices and occasionally journeymen may find themselves unable to meaningfully contribute to the analysis of any of the remaining targets.

# Breadth-first
At the core of the proposed breadth-first targeting strategy is an increase in the number of targets.
Given enough potential targets, lower-skilled hackers are free to jump to a new target as soon as it becomes clear that they cannot make further progress on their current target.
These abandoned targets are then queued to be handled by available journeymen, and likwise by masters if necessary.
To increase breadth even further, the described process stops all work on a target during automated analysis, further increasing the total number of targets that may be analyzed.
The authors provide several ideas for increasing the number of targets, including breaking down targets into multiple smaller targets and transitively considering the dependencies of a target.
Another benefit pointed out by the paper is that a much larger queue of potential targets allows new team members the freedom to select targets compatible with tools they are already familiar with.
The process of handing off targets to more-skilled hackers also provides a feedback mechanism for those attempting to improve their skills: a more skilled hacker will pick up where they left off and document their approach to resolving whatever difficulties forced earlier hackers to move on.
