---
title: "Constraint-guided Directed Greybox Fuzzing"
author: Team Apollo
categories: [Fuzzing]
tags: [Fuzzing, Constraint-Guided, Greybox, Directed]
pin: true
---

## Paper Info
- **Paper Name**: Constraint-guided Directed Greybox Fuzzing
- **Conference**: USENIX Security '21
- **Author List**: Gwangmu Lee, Woochul Shim, Byoungyoung Lee
- **Link to Paper**: [here](https://www.usenix.org/conference/usenixsecurity21/presentation/lee-gwangmu)
- **Food**: No food

## Problem

## Proposed Solution

#### Overview
The solution the paper presents is Constraint-guided directed greybox fuzzing (CDGF), which aims to satisfy a sequence of constraints to better achieve directed fuzzing. CDGF requires a set of constraints, which the paper defines as a combination of a target site to reach in the binary, and the data conditions that must be satisfied at the target site. If a seed causes the program to reach the target site and satisfies all data conditions, the constraint is considered satisfied. The authors keep track of this by capturing all of the variables required to evaluate the constraint when the target site is reached, then they evaluate for the data condition. Usually, multiple constraints are required together, such as the example the author's give in the paper with a use-after-free with two constraints: the first where a pointer is free()'d, and a second site where the pointer is used. If these are satisfied in the given order when evaluating a seed, then the seed is good.
#### Total Distance
The authors also develop a metric to evaluate seeds that they call total distance, which is a combination of the distance to a target site, and the distance of data conditions. The specifics of this system are defined in the paper, a rough outline will be provided for brevity. For the distance of a target site, the minimum distance between the target site's basic block and the execution basic block trace is chosen. This essentially means for a distance of 0, the target site was reached, a distance of max is chosen if it is impossible to reach (e.g. the constraint before was unmet), otherwise the number based on distance (e.g. a distance of 1 means one basic block separated the basic block trace from the target site). For the distance of data conditions, a more complex system is used based on the type of data condition. It's defined such that smaller numbers indicate more data conditions are satisfied, or if the same number of conditions are satisfied, the first unsatisfied data condition is closer to being satisfied. The sum of these is the total distance for one constraint, and the total distance across all constraints is mathematically outlined in the paper, where essentially lower numers are better.
#### Constraint Generation
The authors have also developed a system to generate constraints, which utilize either crash dumps or patch changelogs. They do this with crash dumps by templating constraints to favor common bug types, which they describe as nT (multiple target sites), 2T+D (two target sites + data conditions), and 1T+D (one target site + data conditions). Some common bugs for these types are listed below:

- **nT**: Use-after-free, double-free, use-of-uninitialized-value
- **2T+D**: buffer-overflow (heap and stack)
- **1T+D**: divide-by-zero, assertion-failure

By analyzing crash dumps the system can generate one of these constraint templates for the common bugs outlined above. To utilize patch changelogs, they use the 1T+D template to target locations changed by the patch (intended to fix the crash), where they will analyze for new exception checks, branch condition changes, variable replacements, and a fallback data-condition-free constraint, and template accordingly.
### The CAFL System
Finally, the authors implemented this in CAFL, which accepts source code and the constraints, instruments for edge coverage and target site distances, and then fuzzes the binary and returns the seed distance. The instrumentation is done by generating LLVM IR bitcode, and then annotating the target sites to prevent them from being optimized out. When CAFL begins fuzzing seed distances are reported to the fuzzer via a shared memory interface, and associates them with their respective seeds. Seeds with shorter total distances are prioritized by regulating the selection probability of each seed based on its score. 

## Discussion
