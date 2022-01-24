---
title: "HardsHeap: A Universal and Extensible Framework for Evaluating Secure Allocators"
author: Team Hot Source Rides Again!
categories: [allocators, secure allocators]
tags: [heap, allocators, secure allocators, fuzzing, evaluation]
pin: true
---

## Paper Info
- **Paper Name**: HardsHeap: A Universal and Extensible Framework for Evaluating Secure Allocators
- **Conference**: CCS '21
- **Author List**: Insu Yun, Woosun Song, Seunggi Min, Taesoo Kim
- **Link to Paper**: [here](https://taesoo.kim/pubs/2021/yun:hardsheap.pdf)
- **Food**: No food

# Summary of Reviews and Discussion

# The Evergreen Topic of Novelty

The question of scientific novelty comes up in most of our discussions, and this was no exception.
Different reviews saw the issue very distinctly:
Some thought the paper was clearly novel since this sort of evaluation of secure allocators hadn't been done before; others were of the opinion that it is mostly just an extension of ArcHeap to target secure allocators.

In the past, similar discussions have hinged on the real-world impact of the paper, with people originally skeptical that a paper is truly novel being swayed by sufficient real-world impact.
HardsHeap seems to anticipate this, being very clear in the paper as to which implementation bugs in secure allocators were reported and patched, and including the bug reports as trophies on the project's github page.
While also leading to patches in Markus, isoalloc, and ffmalloc (which was previously discussed [here](https://topicsec.github.io/posts/preventing-uaf-with-fast-forward-allocation/)), it may be the eventual patch to mimalloc with the largest real-world security impact given its use by Microsoft in Bing and Azure.
It's difficult to outright deny the novelty of a new evaluation framework that does directly lead to patching extant vulnerable code, especially given the lack of an existing platform for secure allocator evaluation.

Drawing a hard line between a standalone project and an extension to another project doesn't seem to be easy, but also doesn't seem to be crucial to the question of novelty in this case.

# Structure and Language
