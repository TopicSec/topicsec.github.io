---
title: "How Machine Learning Is Solving the Binary Function Similarity Problem"
author: Team Hawaii
categories: [ ai bindiff binary reverse-engineering ]
tags: [ ai bindiff binary reverse-engineering ]
pin: true
---

## Paper Info
- **Paper Name**: How Machine Learning Is Solving the Binary Function Similarity Problem
- **Conference**: USENIX '22
- **Author List**: Andrea Marcelli, Mariano Graziano, Xabier Ugarte-Pedrero, and Yanick Fratantonio

- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-marcelli.pdf)
- **Food**: Pizza and Bagels

# Paper Recap

As the presence and influence of AI and machine learning in cybersecurity continues to expand, many research areas are obvserving notable improvements from the new techniques. One such area is binary function similarity, which is the problem of taking the binary representation of two functions and producing a number that denotes how "similar" the two functions are. Because we live in a world of varying development environments, traditional numerical comparison methods fail, and more complex methods must be used. This problem is important specifically because the ability to measure similarity between functions is very valuable in reverse engineering, malware analysis, and vulnerability identification. With the amount of existing research in this area, a reader might think that a broad understanding of the field has been reached, however, the authors of the paper in question believe that no current body of published research can arrive at these broad truths. This is because the authors state that all previous work suffers from either an inability to reproduce/replicate previous results or the evaluation results being too opaque. These two problems lead to there being no clear reseasrch direction for binary function similarity, which this paper sets out to solve.

In the paper, the authors perform a measurement study on binary function similarity, and explore existing research with a focus on recent machine learning powered approaches. They also implement a common measurement platform that includes a test suite and a number of existing techniques extended to all of the architectures present in the test suite. They did this by reimplementing existing methodology to create "primitives" of various solutions to compare against, rather than trying to reproduce the results of over a hundred individual solutions. Specifically, they picked 10 approaches and extended or rewrote them to include in their benchmark. A brief summary of the 10 approaches is provided below.

1. Catalog1: 
2. FunctionSimSearch: 
3. Gemini: 
4. ICML19: 
5. Zeek: 
6. ASM2VEC: 
7. SAFE: 
8. Assembly Code Embedding: 
9. CodeCMR/BinaryAI: 
10. Trex: 

