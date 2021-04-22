---
title: "Refining Indirect Call Targets at the Binary Level"
author: Team Pomegranate
categories: [binary-analysis]
tags: [x86, static-analysis, bug-finding]
pin: true
---

## Paper Info
- **Paper Name**: Refining Indirect Call Targets at the Binary Level
- **Conference**: NDSS '21
- **Author List**: Sun Hyoung Kim, Cong Sun, Dongrui Zeng, Gang Tan
- **Link to Paper**: [here](https://www.ndss-symposium.org/wp-content/uploads/ndss2021_2B-4_24386_paper.pdf)
- **Food**: Biscuits and Honey

## What's It About?

It is desirable by both automated and manual bug finders to be able to precisely reason about the control flow of a binary. The most important open research problem in control flow graph recovery is indirect jump resolution, which is tricky because the addresses that control may flow to may be calculated through arbitrarily complex schemes. This paper presents a technique to aid in this process called **BPA**.

The first thing to understand is that BPA is a series of optimizations to a well-known kind of static analysis called a "points-to analysis". This type of analysis has the goal of identifying for each dereference in a program's code, where that memory access could possibly reference. This is a superset of the indirect control flow recovery problem. Points-to analyses are generally understood to be slow because they must reach what is called a "fixed point". This entails running analysis iteratively until no new results can be discovered, a process which asymptotically takes O(n^n) time. In practice, there are a number of optimizations that can be done to make this more tractable - first, analysis can be done on a SSA domain, which makes variable dependencies much better-shaped and accordingly enables parallel analysis, and second, analysis can use datalog, like [this paper](/posts/datalog-disassembly/), which tends to simplify these sort of relational queries.

The main contribution present in BPA is a more flexible memory model for points-to analysis, a **block-based memory model**. This means that once the block scheme is established, any two addresses in the same block can be considered to be identical. Block boundaries are picked by noting any accessed base offset on the stack, heap, or in the global variable store, and then combining any overlapping accesses. Since only base addresses are considered, pointer arithmetic operations can be skipped entirely.

In order to evaluate the effectiveness of this technique, the authors measure its precision, soundness, and efficiency. Precision and soundness must be measured against a ground truth, so the authors collected an approximation of the set of legal indirect call targets by taking a corpus of test cases for each target binary and running them through a version of the binary instrumented to dump all the indirect calls it takes. The soundness metric was a complete success: every single observed indirect call in the corpus was recovered by BPA. Precision was measured in average indirect call targets per callsite, or AICT, allowing techniques to be compared on a per-binary level with a single measurement. This was used to show that when you combine BPA with what is called _arity analysis_, the AICT decreases and the precision increases. Arity analysis is a refinement of the possible call targets based on extracting the number of arguments passed and used at the call site and call target respectively and filtering out any mismatched pairs. The performance evaluation compared the time to run BPA and [BAP](https://github.com/BinaryAnalysisPlatform/bap)'s implementation of [VSA](http://pages.cs.wisc.edu/~bgogul/Research/Papers/cc04.pdf) on the same binary. The findings were that every binary selected takes more than 10 hours for VSA to complete and less than 10 hours for BPA to complete.

## Discussion and Criticism

First, of course, let us draw attention to the age-old problem of not explaining where you get your dataset for dynamic analysis from. It is never explained how the testcases for ground-truth indirect call extraction were generated, and the numbers from these tests are the backbone of their entire evaluation.

Furthermore, the 10 hour threshold used to claim victory over BAP is rather arbitrary, considering that one of the samples took 9.3 hours for BPA to process. In a paper dedicated to the study of tractability, it seems prudent to at least examine where all that time is being spent.

And finally, where is the precision comparison against any other technique? The claim that angr is inapplicable for this kind of evaluation because it "only supports intraprocedural analysis" is patently untrue, and has been untrue for the entire span of angr's existence. This paper desperately needs a precision comparison against _some_ other implementation, be it angr or BAP, in order to tell what if anything is being lost in the quest for tractability. This is the single biggest problem preventing a fair assessment of whether the presented technique actually establishes a real improvement in the state of the art. As it is, the paper in its evaluation gets away with claiming it is inventing the world of indirect call recovery in any sensible timeframe, which is, again, patently untrue.

More investigation is necessary.
