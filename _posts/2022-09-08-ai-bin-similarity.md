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

In the paper, the authors perform a measurement study on binary function similarity, and explore existing research with a focus on recent machine learning powered approaches. They also implement a common measurement platform that includes a test suite and a number of existing techniques extended to all of the architectures present in the test suite. They did this by reimplementing existing methodology to create "primitives" of various solutions to compare against, rather than trying to reproduce the results of over a hundred individual solutions. Specifically, they picked 10 approaches and extended or rewrote them to include in their benchmark. A brief summary of the 10 approaches is provided below (taken from Team Not Yan's presentation).

1. **Catalog1**: IDA plug-in: fuzzy hash, must be same architecture, examines a fixed length byte signature
2. **FunctionSimSearch**: fuzzy hash developed in industry, partial CFG, architectural independent
3. **Gemini**: efficient graph neural network structure utilizing an ACFG with basic block attributes, manually engineered features
4. **ICML19**: graph matching model from Google/deep mind
5. **Zeek**: abstracts to IR, does dataflow analysis, cross-architecture similarity
6. **ASM2VEC**: uses NLP and OOV workaround, unsupervised learning, must be same architecture
7. **SAFE**: sentence encoder built on ASM2VEC to achieve architectural independence
8. **Assembly code embedding**: Gemini enhancement, unsupervised block level features
9. **CodeCMR/BinaryAI**: function level, IR, uses NLP, GNN and basic blocking embedding
10. **Trex**: recent work - extracts functional traces dynamic execution to learn semantics (also utilizes deep learning and NLP)

Then, two datasets were created: Dataset-1 and Dataset-2. Dataset-1 is 7 popular open-source projects including: ClamAV, Curl, Nmap, OpenSSL, Unrar, Z3,and Zlib. Dataset-2 is built ontop of the evaluation set of Trex, and includes 10 libraries built into already compiled binaries for various architectures/optimization levels: Binutils, Coreutils, Diffutils, Findutils,
GMP, ImageMagick, Libmicrohttpd, LibTomCrypt, PuTTy, and SQLite. These two datasets were used to evaluate six different tasks (descriptions taken from paper) across optimization levels (O), compilers/compiler versions (C), bitness (32/64) (B), and architecture (A):

1. **XO**: different optimizations, but the same compiler, compiler version, and architecture.
2. **XC**: different compiler, compiler versions, and optimizations,but same architecture and bitness. 
3. **XC+XB**: different compiler, compiler versions, optimizations, and bitness, but same architecture. 
4. **XA**: different architectures and bitness but the same compiler, compiler version, and optimizations. 
5. **XA+XO**: different architectures, bitness, and optimizations, but the same compiler and compiler version. 
6. **XM**: different architectures, bitness, compiler, compiler versions, and optimization.

Finally, there were three sub-datasets used for the last one, XM:

1. **XM-S**: XM for small sized functions (<20 basic blocks)
2. **XM-M**: XM for medium sized functions (>=20 and <=100 basic blocks)
3. **XM-L**: XM for large sized functions (>100 basic blocks

To evaluate this, the area under curve (AUC) of the receiver operating characteristics (ROC) was calculated, along with two commonly used ranking metrics, mean reciprocal rank (MRR) and recall (Recall@K) at different K thresholds. They also evaluated on a vulnerability discovery task, which covered 8 OpenSSL CVEs and selected two router firmwares, compiling 10 vulnerable functions for four architectures (x86, x64, and the two router firmwares, ARM and MIPS 32 bit). Further results and figures are located in the paper and can be referenced if desired. The provided graphs below are for Dataset-1.

![](/assets/img/2022-09-08-ai-bin-similarity/fig1.png)

Out of the 10 solutions provided, the ICML19 model outperformed all of the other variants on the six evaluated tasks. Some of the other takeaways include that the fuzzy hasing approaches suffer significantly when multiple compilation variables change at the same time, and that the machine learning models broadly outperform the other methods. With this information, the authors answer a number of broad questions about binary function similarity as a research area, especially with regards to machine learning. Specifically, the authors point out that machine learning models achieve high accuracy even when multiple compilation variables change, and benefit from being trained on ground truth examples based on compilation options. In addition,the choice of the type of model and loss function used are as import as properly selecting input features. Models that use basic block features tend to perform better, but more complex input features tend to not outperform simple ones. It's also not necessary to train these models for a specific task, since generic task data from XM achieves results close to tailored training for each task. Future research towards the machine learning side is warranted, with the authors noting there are many models that remained untested, although currently GNN models provide the best results.

 


