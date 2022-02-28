---
title: "Igor: Crash Deduplication Through Root-Cause Clustering"
author: Team Hot Source
categories: [ fuzzing ]
tags: [ fuzzing, crash deduplication, causal analysis ]
pin: true
---

## Paper Info
- **Paper Name**: Igor: Crash Deduplication Through Root-Cause Clustering
- **Conference**: CCS '21
- **Author List**: Zhiyuan Jiang, Xiyue Jiang, Ahmad Hazimeh, Chaojing Tang, Chao Zhang, Mathias Payer
- **Link to Paper**: [here](https://netsec.ccert.edu.cn/publications/ccs21-igor)
- **Food**: Tiramisu

# Paper Summary

While fuzzing is a valuable tool for finding software crashes, these crashes are often not unique and need to be analyzed in order to find the root causes which actually need to be fixed.
Existing approaches to mitigating this problem, such as coverage-based uniqueness or stack hashing, come with their own tradeoffs and can overestimate crashes by orders of magnitude.
Igor is a new tool to address this issue, working in two phases to first flip the fuzzing process on its head and *minimize* coverage in order to trim execution traces as much as possible before deduplication and then use CFG similarity to cluster crashes from there.

## Coverage-minimizing fuzzing

The core of Igor's approach is to attempt to turn all crashes into the shortest possible path to reach that same bug, hopefully removing redundant info or noise from the execution trace.
Existing approaches focus on constraining the size of the crashing input itself rather than the execution trace; for instance, AFL-tmin attempts to minimize the crashing input and even when reducing input size by ~85% leads to ~15% of crashing inputs following *more* CFG edges than before minimization.
Clearly input minimization is far from a perfect proxy for trace minimization, and as such Igor's fitness function favors traces with fewer edges.
Their evaluation of this minimization approach found zero false positives (e.g. a supposedly minimized input actually triggering a different bug) in the minimization of 5,535 PoCs.

## Clustering

Their approach to clustering relies on CFG similarity, computed using the Weisfeiler-Lehman Subtree Kernel.
Unlike graph/subgraph isomorphisms which give binary results on graph similarity, this kernel method outputs a similarity matrix suitable for use in clustering.
The silhouette score, a measure of clustering quality, is used as a metric to evaluate the results.
Spectral clustering relies on knowing the number of clusters in advance, and attempting to compare the results of various cluster counts by silhouette score may still underestimate the true number of clusters.
In this case, manual review of the first-stage clustering may indicate another pass of clustering is in order.

## Evaluation
Running IgorFuzz for their recommended default cutoff time of 15 minutes, beyond which they observed diminishing returns, led to a 14.4% improvement in silhouette scores.
While not the easiest to interpret, this does represent substantial improvment to crash clustering.
Figure 7 from the paper illustrates crash clusters before and after test case reduction using Igor and is far more intuitive than interpreting the data on silhouette scores.

![](/assets/img/2022-03-02-igor-crash-deduplication/fig7.png)

# Preliminary Reviews

|            | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------- | ------ | ------ | ------ | ------ | ------ | ------ |
| Review #4A | 4/6    | 2/4    | 3/5    | 2/3    | 3/5    | 1      |
| Review #4B | 5/6    | 2/4    | 4/5    | 2/3    | 4/5    | 1      |
| Review #4C | 5/6    | 3/4    | 3/5    | 2/3    | 4/5    | 1      |

# Discussion

## ML Techniques

## Resource Usage
