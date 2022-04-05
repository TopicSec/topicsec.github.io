---
title: "Automated Bug Hunting With Data-Driven Symbolic Root Cause Analysis"
author: Team Hot Source
categories: [ bug detection, data analysis ]
tags: [ bug detection, data analysis, root cause analysis ]
pin: true
---


## Paper Info
- **Paper Name**: Automated Bug Hunting With Data-Driven Symbolic Root Cause Analysis
- **Conference**: CCS '21
- **Author List**: Carter Yagemann, Simon P. Chung, Brendan Saltaformaggio, Wenke Lee
- **Link to Paper**: [here](https://dl.acm.org/doi/pdf/10.1145/3460120.3485363)
- **Food**: Lemon bars

# Paper Summary
This paper presents Bunkerbuster, a system for automated data-driven bug hunting of memory corruption bugs using symbolic root cause analysis. It is an automatic binary-only vulnerability discovering tool that can trigger 39 vulnerabilities in 15 programs. Out of these 39, 8 are 0 day vulnerabilities, one EDB and 3 CVE IDS. Since it is not dependent on human-written harnesses, it requires even less human intervention compared with fuzzers. Bunkerbuster uses snapshots and Intel-PT to collect information on running processes. Based on the given information, it reconstructs program states in a symbolic execution engine and performs under-constrained symbolic execution to find new bugs.

![](/assets/img/2022-04-05-bunkerbuster/bunkerbuster.png)

Due to the use of snapshots, Bunkerbuster does not need to explore all the program states at once. Instead, it only explores a small portion of the code that can be traversed by symbolic library calls. This technique significantly reduces the potential search space and avoids the infamous path explosion problem in symbolic execution. That said, Bunkerbuster has some limitations. The targeted programs are limited to benign, unobfuscated, Linux binaries and the system can detect bugs like overflow, UAF, DF and FS.Moreover, bugs found by API snapshots may not be reachable.

# Review Summary
|            | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------- | ------ | ------ | ------ | ------ | ------ | ------ |
| Review #9A | 2/6    | 4/4    | 2/5    | 3/3    | 4/5    | 2      |
| Review #9B | 2/6    | 3/4    | 2/5    | 2/3    | 3/5    | 2      |
| Review #9C | 3/6    | 1/4    | 4/5    | 2/3    | 4/5    | 2      |

The through-line of reviews was criticism of the overall framing of the paper as more industry-targeted. It seems like with every other paper we revisit the question of *is finding real-world bugs enough?*; typically those concerns are outweighed by defenses of the scientific contribution of the paper. This stood out more than usual for this paper due to the core techniques of Bunkerbuster (use of traces, snapshots, and symbolic execution) existing in published work. This is hardly an uncommon topic of discussion: these reviews echo previous ones in that the framing of a paper can come to dominate the discussion over its objective contributions. As such, considering this angle during the writing process is critical. The most positive framing of results for one audience may be regarded as obscuring precisely the aspects a different audience values.

# Discussion Summary

Discussion of the paper largely revolved around its supposed novelty and evaluation. The primary criticism of Bunkerbuster was of its composite nature - at first glance the tool is largely a combination of various existing techniques instead of anything new. While Bunkerbuster did find bugs via symbolic execution that fuzzing platforms like AFL and QSYM could not, the scientific contribution was not obvious to the reviewers. Further discussion placed this as a framing problem: reviewers felt that Bunkerbusters' actual contribution lay more in the use of snapshot+traces to manage path explosion than the engineering done on the framework itself. As written, the papers' introduction and evaluation diluted the apparent impact of this contribution. Thus, reviewers ended up focusing on the system, its privacy concerns, its engineering feasibility and similarity to other existing tools instead, making it difficult to distinguish Bunkerbusters' contributions from existing techniques.

As a minor note, the paper's diagrams and images can be improved, such as Figure 4 which is placed sideways instead of upright, making it basically impossible to read normally.
