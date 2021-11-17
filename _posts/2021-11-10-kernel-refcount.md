---
title: "Detecting Kernel Refcount Bugs with Two-Dimensional Consistency Checking"
author: Team bi0s
categories: [Linux-Kernel]
tags: [Linux Kernel, refcount, resource sharing]
pin: true
---

## Paper Info
- **Paper Name**: Detecting Kernel Refcount Bugs with Two-Dimensional Consistency Checking
- **Conference**: USENIX Security '21
- **Author List**: Xin Tan, Yuan Zhang and  Xiyu Yang
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec21-tan.pdf)
- **Food**: No food

## Problem

Linux kernel relies on refcount to track shared objects. This is achieved by the atomic increment and decrement functions atomic_INC/DEC/SET(atomic_set sets refcount to 1). There are two potential problems associated here. One is missing refcount which can lead to memory leak and the other is over-decrease of refcount, which can lead to “use after free” bug which is a security threat.

## Solution Design

First they identified all the structures with a refcount field by converting kernel code to byte code using a LLVM compiler which gave around 3220 refcount fields. Then they manually analyzed 300 odd fields and analyzed the unique behaviour of the refcount field operated  by SET, INC, DEC. The assumption is that the INC is in callee function and DEC is in caller function. They do a call graph analysis to identify INC/DEC call pairs. The idea is to find INC first by following the call graph path. Then they trace back to find the corresponding DEC in the parent path.

### Consistency Checking

There are two patterns to isolate inconsistencies.

**Pattern1**: For every INC function, trace the corresponding DEC and analyze the path for any inconsistency that can lead to unbalanced refcount.

**Pattern2**: For every INC find the corresponding DEC pair. Any case where INC and DEC do not exist in pairs is likely a bug.

## Evaluation

For the evaluation, the authors analyzed 19.2 million lines of code in the Linux kernel version 5.6-rc2. This was performed on a machine with 128 GB of RAM and two processors with 20 cores each.
This process took 18 minutes and generated 149 bug reports.
The authors manually analyzed the 149 bug reports which took them 37 man hours.
Of the 149 bug reports, 44 were new refcount bugs and 105 were false positives.
36 of these bugs have been confirmed with 34 of them already fixed.

A majority of the false positives bug reports were caused by the inaccuracy of the static analysis. The authors however discuss possible directions to mitigate these false positives by applying more precise analysis techniques.

### Comparative evaluation

The authors compared CID with RID which is the existing state-of-the-art and closely resembles the concept used by CID.
In order to compare these two approaches, the authors selected 60 refcount bugs reported between 2018 and 2020.
CID was able to detect 54 (46 unique) bugs whereas RID was only able to detect 10 (2 unique) bugs.

### Evaluating refcount field identification

Since a key contribution of CID is its ability to identify refcount fields, the authors evaluated this aspect.
CID identified 792 refcount fields in the Linux kernel.
The authors had previously analyzed a random sample of 300 fields and classified them into **Lock/Status** (**18**), **Token/ID** (**16**), **Normal Counter** (**121**) and **Refcount** (**145**).
Therefore, they had the ground truth information for 300 cases where 145 are positives and the remaining 155 are negatives.
The authors note that the highest accuracy obtained is close to 94% when identifying refcount fields.


## Discussion

The discussion started off by talking about the importance of quality of writing in a paper. We talked about how the core quality of the paper lies in the idea and not the way it is written but the quality of writing is very important, sometimes papers get rejected for bad writing and sometimes people get lucky. We found that this paper was badly written as we had difficulty in understanding the technical details. This paper surely deserves to be a top tier paper but it would have been even better if the writing was improved a bit.

The next question raised was whether it is a good practice to squeeze two mediocre projects into one top tier paper. Sometimes people submit mediocre papers to a top tier conference to get an experience with the writing and the review process. Sometimes in labs there is an intern project lying around and they merge it into a major paper that is being submitted. This paper tries to present the concept in a two-dimensional way which seems like two different projects based on pattern analysis. Comparing this to *Adam's* (paper)[https://www.researchgate.net/publication/306306274_SoK_Everyone_Hates_Robocalls_A_Survey_of_Techniques_Against_Telephone_Spam] - major revision was to remove half the paper as the paper took two studies together and combined to put it into the paper.  The consensus was that it is a dangerous strategy that rarely works so you probably shouldn't start out with that idea.
