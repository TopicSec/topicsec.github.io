---
title: "MirChecker: Detecting Bugs in Rust Programs via Static Analysis"
author: Team Thanos was Right
categories: []
tags: []
pin: true
---

## Paper Info
- **Paper Name**: MirChecker: Detecting Bugs in Rust Programs via Static Analysis
- **Conference**: CCS '21
- **Author List**: Zhuohua Li, Jincheng Wang, Mingshen Sun, John C.S. Lui
- **Link to Paper**: [here](https://dl.acm.org/doi/abs/10.1145/3460120.3484541)
- **Food**: Black Forest Cake


Paper summary
Rust is a popular programming language designed with safety in mind. Its rigorous type and unique ownership system can help developers avoid many memory-related issues during compile time. However, even though Rust is considered a safe language, it is not a panacea. Rust programs still suffer from two major vulnerability categories: runtime panics and lifetime corruption. The former are usually caused by incorrect manipulation on integer values, thus leading to triggering runtime assertions inserted during compilation time. The latter involves the “unsafe” keyword in Rust. Incorrectly using “unsafe” can break the ownership system and lead to use-after-free vulnerabilities.

In this paper, the authors designed and implemented a tool, called MirChecker, to statically identify the two bug categories in Rust programs. The tool works on Rust’s Mid-Level Intermediate Representation (MIR) to work on its simple syntax and utilize its type info and debugging data. It is implemented as a customized Rust compiler so that it can find bugs during compilation time. In their design, they introduced a simple language model to capture the core syntax of Rust MIR. The language defines “abstract domains” and “transfer functions” and it can be used for construction of symbolic expressions to simulate memory accesses. The tool analyzes programs at function level and treats each program as a Control-Flow-Graph (CFG). It then traverses the CFG to perform numeric and symbolic analyses. Finally, it will pass the collected information to SMT solvers to verify some predefined security conditions. Warnings will be generated if SMT confirms that security conditions are violated.

The authors evaluated their tool on more than 1000 realworld Rust crates and discovered 17 runtime panics and 16 memory-safety bugs. The result was obtained with a false-positive rate of 95.1%. The high positive rate is caused by three factors: 1. the nature of the static analysis will inevitably introduce imprecision 2. Many reported warnings are caused by panic calls injected by developers, thus not bugs 3. A single bug can be triggered through different paths.  According to the authors’ assessment, most of the bugs do not lead to severe vulnerabilities, which confirms the safety of Rust. 
Discussion
The discussion regarding this paper was relatively short and centered around what could have been the reasons for this paper being accepted. One of the most prominent issues that standout at a first glance is the high false positive rate. It prompts the question as to whether a paper that performs a similar type of analysis on C++ or on the Juliet data set with a similar false positive rate would’ve been accepted or not.

However, the novelty in this paper stems from its application on Rust which is considered to be relatively safer among programming languages. The static analysis techniques described in the paper are not very novel by themselves, but their application to verify the claim of security attributed to software written in Rust is a novel contribution.

A similar pattern can be seen in papers that were aimed at detecting vulnerabilities in web applications that used a known set of static analyses to find a new class of bugs in a new domain.

Majority of the reviewers agreed that the paper was well written and had many merits. Ultimately what decides whether a paper gets accepted or not is the weighing of merits vs demerits. And in conclusion, the only downside for this paper seems to be their high false positive rate.
