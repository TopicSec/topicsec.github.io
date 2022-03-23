---
title: "Learning to Explore Paths for Symbolic Execution"
author: Team paper cuts
categories: [ binary-analysis ]
tags: [ static-analysis, bug-finding, machine learning ]
pin: true
---

## Paper Info
- **Paper Name**: Learning to Explore Paths for Symbolic Execution
- **Conference**: CCS '21
- **Author List**: Jingxuan He, Gishor Sivanrupan, Petar Tsankov, Martin Vechev
- **Link to Paper**: [here](https://dl.acm.org/doi/abs/10.1145/3460120.3484813)
- **Food**: oatmeal raisin pecan cookies

# Paper Summary

The primary limitation of the effectiveness of symbolic execution is due to the path explosion problem, the potential exponential growth of states.  Techniques such as applying heuristics have attempted to minimize this issue, but only been met with limited success. another technique is to utilize machine learning.  The authors have  developed a program known as Learch that utilizes iterative learning to direct the exploration of promising states. 

![](/assets/img/2022-03-23-learning-to-explore-paths-for-symbolic-execution/1overview.png)

Learch first examines the source code of a program and extracts potential states and retrieves the state information which it uses as features for the neural network.  The neural network provides the answer to the question of if the state is worth exploring or not, and utilizes coverage data and heuristics in training data and feature extraction.  Features are a combination of heuristics used in other research as well as a number of new ones.  Learch is basically implemented as a searching agent in KLEE and utilizes Python/Pytorch as the framework for their learning model.

![](/assets/img/2022-03-23-learning-to-explore-paths-for-symbolic-execution/2dataset-generation.png)

Training this neural network requires the creation of a dataset built with supervised learning.  This is done by generating a control flow graph of the program, and for each corresponding state, recording the time spent in each state by the symbolic engine.  The reward for each state is the resulting coverage divided by time spent.   

![](/assets/img/2022-03-23-learning-to-explore-paths-for-symbolic-execution/3evaluation.png)

Two evaluations were performed.  The first evaluation only utilizes symbolic execution and utilized the modules of Coreutils and ten other real-world binaries.  The second evaluation took the 4 largest programs from the first evaluation, and used a sanitizer (UBSan) to determine the line coverage and total UBSan violations that could be found in these programs.  This was done by seeding the AFL fuzzing engine with test cases produced by symbolic execution.  Learch was then compared against six of the other state-of-the-art solutions and produced top results in all cases.

Learch results in a decent improvement over existing techniques.  It shows an even greater improvement of roughly 30% over existing state-of-the-art techniques for programs with larger file sizes, and can incorporate improvements in heuristics immediately as part of its own solution.  The research evaluation with Learch also generated a number of bug reports - 258 violations were detected, the authors filed 46 bug reports, and 11 of them were fixed.

# Preliminary Reviews

|            | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------- | ------ | ------ | ------ | ------ | ------ | ------ |
| Review #4A | 5/6    | 2/4    | 3/5    | 3/3    | 3/5    | 1      |
| Review #4B | 4/6    | 4/4    | 3/5    | 3/3    | 3/5    | 2      |
| Review #4C | 3/6    | 3/4    | 3/5    | 2/3    | 4/5    | 1      |

# Discussion

The primary strength of this research seems to be in its detailed evaluation. The idea of applying a neural networks to a decision-making process such as symbolic engine branching is trivial, the difficulty is in the implementation. In particular, the KLEE implementation performed is very impressive.

An interesting omission is that nowhere in the paper is it stated that the bug reports originated only from violations that were detected by Learch.  These bugs theoretically could be discoveries made by the improved code coverage of the tool or simply the result of running a fuzzer on multiple software products for an extended period of time.

This brings up interesting question about how important bug discoveries are to the success of a paper such as this one.  The main proof of an innovative approach is that it improves upon the existing state-of-the-art.  The discovery of software bugs through the use of an new tool by itself should not be enough to get a paper accepted, but rather a significant theoretical advancement in science that is proven by its ability to find previously unfindable bugs.  Of course, if you are finding bugs that have previously not been found even with what appears to be a very trivial technique, this still testifies to its innovative approach.  Finding bugs is a type of proof that your tool is effective, but it should be a clear result of what actions you take.  Another important element is bug severity.  Tools that can find remote code execution bugs are much more useful and interesting than bugs that are simply denial of service or trivial UBSan violations for example. 

Learch's effectiveness was primarily evaluated in terms of number of code pathways found, with an interesting 2nd metric of number of the UBSan violations detected.  It would also have been useful to know how quickly the paths were found when compared to traditional methods.  The use of other alternative metrics such as performance overhead would be useful indicators to help judge how much of a positive impact might make in the real world.
