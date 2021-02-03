---
title: "Devil is Virtual: Reversing Virtual Inheritance in C++ Binaries"
author: Team p-mango
categories: [binary-analysis]
tags: [c++, oop, virtual-inheritance]
pin: true
---

## Paper Info
- **Paper Name**: Devil is Virtual: Reversing Virtual Inheritance in C++ Binaries
- **Conference**: CCS '20
- **Author List**: Rukayat Ayomide Erinfolami, Aravind Prakash
- **Link to Paper**: [here](https://arxiv.org/pdf/2003.05039.pdf)
- **Food**: Cookies

## Prerequisites

There's no way around it: you need to understand the nitty-gritty details of C++ virtual inheritance.
[This stackoverflow answer](https://stackoverflow.com/a/16097013) does a much better job than we could of explaining it all.

## Problem

Most static analysis tools for C++ have focused on reversing single and multiple inheritance rather than virtual inheritance.
While it's certainly not as common as the other types of inheritance, in their sample the authors found that virtual inheritance was used in 11% of the Linux binaries and 12.5% of Windows binaries.
This is a nontrivial portion of real-world programs, and this lack of support for virtual inheritance has security implications.
Current defenses against virtual dispatch attacks suffer from either false positives or false negatives in the presence of virtual inheritance.

Additionally, the recovery of virtual inheritance is simply more difficult than that of single and multiple inheritance.
Due to deduplication of virtual base subobjects, the offset to the virtual base varies based on which specific subobject is being considered.
All of these must be accounted for in order to properly ensure CFI in the case of virtual dispatch attacks.

## Solution
In summary, this paper addresses the problem of reversing virtual inheritance in C++ binaries. This research question is meaningful, since from examples presented in this paper, the failure of detecting virtual inheritance can tigger false positives and false negatives. VirtAnalyzer is proposed to solve this question which can recover virtual inheritance with a decent detection rate in the virtual inheritance tree.

The high-level ideas behind VirtAnalyzer lie at different aspects. The first is to recognize the relationship between mandatory and optional fields, this can be done by combining different VTables. Next, identifying construction VTables is also necessary since VTT is important in virtual inheritance as well as discriminating construction VTables and regular VTables. Then, based on the fact that the virtual function’s regular VTable is the same, grouping construction VTables of a class can be done. The last is to identify virtual and non-virtual bases so that the model can filter out non-virtual bases.

To be more specific, there are two phases in VirtAnalyzer, the first phase is to extract metadata from binaries and the second phase is to recovering bases. For the first phase, there are also two different focuses including identifying certain components and building relationships. The identification process mainly includes identifying VTables, VTTs, and subVTTs. After identification, virtual base offsets need to be extracted, in order to prevent recovering false VTables, there should be at least one matching virtual base offset and offset-to-top, otherwise, the VTT need to be discarded. Then, construction VTable is mapped to regular VTables. At last, constructors and destructors are identified and parsed. For the second phase, with merging recovered virtual bases and intermediate bases, the virtual inheritance tree is built of binaries. The workflow of VirtAnalyzer is displayed below.

![](/assets/img/2021-01-27-devil-is-virtual/workflow.png)

## Discussion

This paper sparked the question of how to evaluate the recovery of class info, should we see how close we can get to recreating the source code, or should we evaluate against the available decompilation tools and see how the information of classes can be used to make reverse engineering of programs more manageable. There was also a discussion on how much background info a technical paper should have, especially when you know that the target audience is tiny. Another question was where to draw the line between an engineering paper and a novel research paper, the consensus that it is acceptable to see an engineering paper once in a while as long as it helps science move ahead.

The take away from this paper was that even if the advancements are small or have a small target audience (reverse engineers, hardcore C++ programmers), as long as you can reimplement the engineering work on a functioning tool (e.g., IDA or GHIDRA) and improve program analysis, the research is considered to be impactful.
