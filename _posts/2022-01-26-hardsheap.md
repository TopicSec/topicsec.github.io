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

HardsHeap presents a method/framework for evaluating secure allocators via integration with a fuzzer (AFL). It builds upon previous work by ArcHeap and successfully found 10 bugs in 7 allocator implementations. This article aims to summarize and discuss the criticisms made by other review groups of the papers' strengths and weaknesses. As a whole, the 4 review groups were highly positive on HardsHeap's acceptance into a top-tier security conference and its relative impact on its field. However, there were a number of specific criticisms on (1) the scientific novelty of the paper and (2) the quality of its presentation. This article begins with an overall discussion of the various strengths and weaknesses collated from the submitted reviews so far before going into detail about points (1) and (2) respectively.

| 			 | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------  | --- | --- | --- | --- | --- | --- |
| Review #1A | 4/6 | 1/4 | 4/5 | 2/3 | 3/5 | 1 (N/A) |
| Review #1B | 6/6 | 2/4 | 4/5 | 3/3 | 4/5 | 1 (N/A) |
| Review #1C | 5/6 | 3/4 | 3/5 | 3/3 | 4/5 | 1 (N/A) |
| Review #1D | 5/6 | 4/4 | 3/5 | 2/3 | 3/5 | 1 (N/A) |

Reviewers spanned the range of expertise on the topic of secure allocators, with one reviewer in each level of expertise from 1 to 4. Despite this, expertise had little effect on interest and acceptance, with all four review groups choosing to accept the paper (two accepts, one weak accept, and one strong accept). We see that while the paper was ranked with an above average amount of merit and strong reviewer interest, reviewers generally ranked the writing quality lower than the overall merits of the paper. Correlating this with the resulting recommendations and feedback, we suspect that the relatively weak presentation of the paper is the main concern holding back full understanding and acceptance.

Group discussion between reviewers generally agreed that the core merit of HardsHeap is in how the paper tackles an important, if obvious, area in secure allocator research. Previous papers in the field had yet to adequately propose a means of objectively evaluating new secure allocator designs. Musings about scientific novelty aside, reviewers also praised the impact of the paper as a primary reason for its acceptance, with two groups referencing the real-world "trophies" found by HardsHeap on their GitHub repository as a significant factor in their appreciation of the paper. On the flip side, multiple reviewers questioned the true extensibility of the platform, citing its lack of support for Windows and other non-standalone secure allocators, as well as the relative effort of "only requiring hundreds of lines [of code]" to implement new modules. Besides this, reviewers debated heavily on the merits and fairings of the paper's presentation style, overall finding it adequate for communicating its overall ideas but poorly focused when it came to implementation details the reviewers were interested in.

# The Evergreen Topic of Novelty

The question of scientific novelty comes up in most of our discussions, and this was no exception.
Different reviewers saw the issue very distinctly:
Some thought the paper was clearly novel since this sort of evaluation of secure allocators hadn't been done before; others were of the opinion that it is mostly just an extension of ArcHeap to target secure allocators.
To the point of those viewing it as more of an extension, a common point among reviewers was questioning how the fuzzer itself was concretely used in HardsHeap.
In some ways, familiarity with those details from ArcHeap seemed to be assumed to the detriment of readers who may not have been familiar with it.

In the past, similar discussions have hinged on the real-world impact of the paper, with people originally skeptical that a paper is truly novel being swayed by sufficient real-world impact.
HardsHeap seems to anticipate this, being very clear in the paper as to which implementation bugs in secure allocators were reported and patched, and including the bug reports as trophies on the project's github page.
While also leading to patches in Markus, isoalloc, and ffmalloc (which was previously discussed [here](https://topicsec.github.io/posts/preventing-uaf-with-fast-forward-allocation/)), it may be the eventual patch to mimalloc with the largest real-world security impact given its use by Microsoft in Bing and Azure.
It's difficult to outright deny the novelty of a new evaluation framework that does directly lead to patching extant vulnerable code, especially given the lack of an existing platform for secure allocator evaluation.
On a more meta level, we discussed the ability of program committees to champion a paper in order to shine a spotlight on a research area; being in a newer or underexamined area in the first place can be to the paper's own benefit in this regard.

Drawing a hard line between a standalone project and an extension to another project doesn't seem to be easy, but also doesn't seem to be crucial to the question of novelty in this case.

# Structure and Language

The **writing style** of the paper was not reader-friendly, and it might seem overcomplicated for some readers. The grammar and the formatting could be improved, there were a lot of typos in the writing.

Moreover, **the tables and the diagrams** were hard to follow, and it seemed that they did not help to explain the concept well. It seemed that some illustrations were not necessary to explain the concept. The illustrations appear in places where it does not make sense but some of it is due to the fact that Latex does the formatting nicely, but it places the illustrations in a place where it will utilize the space efficiently.

The **related works** is in section 11 which appears much later in the paper comparatively. This might be due to the fact that in systems papers, they are placed towards the end of the paper because they do not really understand the system that they have built. So it is hard to have a related works section comparing their work with their own work. It is common to have the related works section towards the end of the paper in systems but if there is any work upon which their system builds, like Archeap in their case, it needs to be mentioned earlier in order to describe their design.

Furthermore, while presenting their **pseudo-code**, they have used Python unlike the regular practice.

To summarize it, as we have discussed - the paper is like a master class in how to take a relatively simple idea that was not explored before and present it as a new idea.

# Bonus Round: What Are You Doing To That Fuzzer