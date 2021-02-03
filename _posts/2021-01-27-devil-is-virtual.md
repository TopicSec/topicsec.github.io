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

## Discussion
