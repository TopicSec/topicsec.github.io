---
title: An Observational Investigation of Reverse Engineers’ Processes
author: Team Fable
date: 2020-10-28
categories: []
tags: [bug-finding, ]
pin: true
---

## Paper Info
- **Paper Name**: An Observational Investigation of Reverse Engineers’ Processes
- **Conference**: USENIX Security '20
- **Author List**: Daniel Votipka, Seth Rabin, Kristopher Micinski, Michelle L. Mazurek
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec20-votipka-observational.pdf)
- **Food**: Pairs well with soft pretzels

# Introduction

Towards a better understanding of reverse engineers’ processes, this paper presents a semi-structured, observational interview study of reverse engineers, with the goal of producing insights for improving interaction design for reverse engineering tools.

In particular, they set out to answer the following research questions:

- RQ1. What high-level process do REs follow when examining a new program?

- RQ2. What technical approaches (i.e., manual and automated analyses) do REs use?

- RQ3. How does the RE process align with traditional program comprehension? How does it diﬀer?

They used an exploratory qualitative approach based on prior work in expert decision-making and program comprehension. To this end, they conducted a 16-participant observation study. For participants, they were asked to recreate RE experience while were observed to track the process of decision making, hypotheses formulated, and beacons(recognizable patterns) identified.

In results, they modeled the RE process to three phases:

- Overview (of the program's functionality)

- Sub-component scanning(specific useful components)
- Focused experimentation(execution/debugging or in-depth static analysis)

Based on these results, the paper suggest five guidelines in the end for designing RE tools.

# Background

## Naturalistic Decision-Making

NDM research focuses on these decision-making processes and uses interview techniques designed to highlight critical decisions, namely the Critical Decision Method, which has participants walk through speciﬁc notable experiences while the interviewer records and asks probing follow-up question. They apply the Critical Decision Method to guide the investigation.

## Program Comprehension

Program comprehension research investigates how developers maintain, modify, and debug unfamiliar code similar problems to RE. Programmers’ hypotheses are based on beacons recognized when scanning through the program. To evaluate their hypotheses, developers either mentally simulate the program by reading it line by line, execute it using targeted test cases, or search for other beacons that contradict their hypotheses. They anticipated that REs might exhibit similar behaviors, so they build on this prior work by focusing on hypotheses, beacons, and simulation methods during interviews. At the same time the paper also hypothesized some process divergence, like RE's targets are usually obfuscated code and raw binaries, and focusing on exploiting flaws.

# Method

The paper employ a semi-structured, observation-based interview protocol to study RE experts' process.

They implemented a modiﬁed version of the Critical Decision Method, which is intended to reveal expert knowledge by inquiring about speciﬁc cases of interest. They asked participants to choose an interesting program they recently reverse engineered, and had them recall and demonstrate the process they used. Each observation was divided into the two parts: program background and RE process.

- Program background: describe the program, includes functionality, size and RE tools they used.

- Reverse engineering process: program-speciﬁc RE goals and questions, and recreating their process with sharing screen.
  - Decisions: including deciding whether to delve deeper into a speciﬁc function or which simulation method to apply to validate a new hypothesis
  - Hypothesis: why they think the hypothesis might be true and how they tested it

  - Simulation methods: to evaluate any tools like debuggers to discuss their effectiveness
  - Beacons: how REs perform pattern matching, and identify potentially common beacons of importance

## Data analysis

After organize interview data into an initial codebook, they develop a theoretical model by: first, grouping identiﬁed codes into three categories: Overview, Sub-component Scanning, and Focused Experimentation. Then they performed an axial coding to determine relationships between and within each phase and trends across the three phases, and derived a theory of REs’ high-level processes and speciﬁc technical approaches.

## Participants

They recruited interview participants from online forums, vulnerability discovery organizations, and relevant conferences.

![](/assets/img/participants.png)

# Results: Trends Across Phases

The authors also noted several trends that spanned the different phases; these comprised both answers to initial research questions that simply weren't phase-specific as well as further observations.

The first trend was beginning with static analysis and finishing with dynamic analysis.
The initial two phases primarily focused on static analysis before transitioning to dynamic techniques for the focused experimentation phase.
Of note was the potential for this switch from static to dynamic analysis to disrupt work, as information needed to be manually transferred over.
Using both static and dynamic tools side-by-side was one approach to alleviating this issue.

The next trend was the tendency for experience and learned strategy to guide the first two phases.
At the beginning, there isn't a defined scope of exploration, and so prior experience is used to bridge this gap.
In a familiar area, one may have some standard process; one participant noted a prioritized list of different APIs to check when dealing with iPhone vulnerabilities.
In the absence of this experience, general search strategies take over, such as prioritizing larger functions as probably more interesting.
Later, in the second phase, this experience again helped participants form hypotheses based on knowledge of previous vulnerabilities.

Another observed trend was the use of experience to select analysis methods.
Some participants would forgo the first phase entirely and begin directly with the `main` function.
Another common observation was knowledge of tool limitations enabling participants to second-guess potentially buggy results; specifically, one participant noted the importance of realizing the Hex-Rays decompiler is buggy rather than trusting its output directly.

Another observed trend dealt with tool selection.
The participants' preferred tools allowed results to be mapped back onto specific lines of code, such as viewing the strings of a binary in IDA rather than using `strings`.
One participant was noted to use a method which did *not* allow for this (manually mapping back and forth between a symbolic representation and the program itself), but explicitly stated that this was only due to not knowing a tool which could accomplish it in that instance.

Another broad trend was a focus on improving the readability of code.
This could involve the use of a decompiler or other methods to improve the usefulness of assembly.
Most participants were observed to rename variables, reconstruct data structures, and explicitly take notes as a result of their analysis.
One participant specifically called out the case of several manipulations of a single variable as requiring notes to keep track of.

Finally, the last cross-phase trend observed was a tendency to query online resources in order to aid analysis.
System and API documentation was widely used, as were third-party breakdowns of the poorly documented aspects.
If these approaches failed, some participants searched StackOverflow hoping the issue had been encountered before, or searched for unique constants in the algorithm they were analyzing (e.g. searching the initialization vector for MD5 reveals the algorithm).
