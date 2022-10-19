---
title: "Decomperson: How Humans Decompile and What We Can Learn From It"
author: Thanos Was Left
categories: [Reverse Engineering, Decompilation]
tags: [reverse engineering, decompilation]
pin: true
---

## Paper Info

- **Paper Name**: Decomperson: How Humans Decompile and What We Can Learn From It
- **Conference** USENIX '22
- **Author List**: Kevin Burk, Fabio Pagani, Christopher Kruegel, and Giovanni Vigna, UC Santa Barbara
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-burk.pdf)

### Introduction
Reverse engineering is a key step to perform security assessment on software binary code. Usually, security researchers will try to turn binary code back into its original form, the source code, first for further analysis, such a finding vulnerabilities. The process is called decompilation. Many works have been done on improving decompiled results provided by tools. But none studied how human experts perform the decompilation process and reverse engineering tasks. This work, Decomperson, tries to fill the gap. It proposes to use the difference between the “perfect decompilation” and the code recovered by reverse engineers to measure their understanding on the target program, thus providing an objective metric for us to study how human experts perform reverse engineering tasks.

### Background

Decompilation is the process of turning a program binary back to its source code form. Due to information loss during compilation process (for example, variable names), the recovered code is usually different from the original source code. Sometimes, the recovered code is not even in real program language but in pseudo code. Due to the difficulty of decompilation, many works have proposed different advanced techniques to improve the accuracy of existing decompilation tools.

Perfect decompilation is a concept proposed in this work. It means the process of recovering, from binary programs, source code, that when compiled, produces exactly the same binary as the original binary programs. In other words, perfect decompilation produces source code that may be different from the original source code but having exactly the same functionality as the original program.

### System Design

The high-level workflow of DECOMPERSON is the following: 
1. Users edit the source code using DECOMPERSON interface; 
2. The source code is sent to the back-end component, where it is compiled, disassembled, tested, and diffed against the original binary; 
3. THe binary is scored based on different metrics, and the results, including the diff, are sent back to the interface.

There are three important components of DECOMPERSON:

#### The Interface
The user interface is the key of DECOMPERSON system, it is web based and it provides **diff** view, which indicates reverse engineers exactly where their binary does not match the target, providing immediate feedback on the effects of any source code chang. Apart from that, it can also show the corresponding relation of source code and assembly codes.

#### The Disassembler
In order to keep diffs usable, DECOMPERSON needs assembly that was free of any noise that would lead to spurious highlighting. Considering the main source of noise are the absolute memory address, DECOMPERSON includes a custom disassembler that replaces absolute memory references with symbols where possible, or generic identifiers where not.

#### The Back-end
When the “compile” button is clicked, the source code is sent to the DECOMPERSON back-end for future analysis.

###  Evaluation

The data analysed in this paper is from an online competition called DECOMPETITION, which is a competition purely focused on reverse engineering.

In order to study the process that humans use when decompiling, all submissions made during DECOMPETITION are recorded. There are a total of 35,530 source code submissions, 25,951 (73%) of which successfully compiled. The majority of the submissions were for challenges written in C, C++, and Go. With about 10,000 submissions each, these three languages account for almost 83% of all submissions. Rust and Swift were less popular, with around 3,000 submissions per language, likely because of their perceived difficulty. With those large-scale data, the following research questions need to be answered.

#### Can Humans Perfectly Decompile?
Based on all submissions, a total of 91 players reached a perfect score on at least one challenge. This is only 48% of the 188 users who submitted source code, but 57% of the 159 users who persisted until their code compiled. Also, More than 75% would go on to reach a perfect score on at least one challenge.

These results shows that perfect decompilation is a reasonable standard of program comprehension, reachable even by non-experts.

#### How do Humans Decompile?

There are some metrics about analysing participants’ submissions, including code and score changes, function focus, and edit classification. The following patterns can be drawed.

Reversers prefer to make frequent, small code changes, submitting often and refining their mental model of the program based on the resulting assembly changes. Experience is useful, particularly for higher-level languages, allowing reversers to quickly map common assembly patterns to the corresponding source code patterns. 

Reversers tend to start work by recreating an outline of the program, providing stubs for all the important functions—a task that could be made easier in a dedicated function stubbing view with a built-in demangler. They then refine these functions one at a time, typically in an order based on the control flow graph. 

Within each function, there were two main challenges: to replicate the behavior of the function and to make sure the local variables are in the correct stack slots. These tasks could also be streamlined with dedicated views: a graph-matching algorithm could be used to generate local variable aliases (instead of stack offsets) for use while working on functionality, and variable declarations could likely be reordered automatically to give the correct stack layout. The reversers’ concentration on single functions also suggests that humans are able to generate perfect decompilation without relying on the interprocedural context used by automatic decompilers. 

#### Does Reversing Equal Decompilation?

Decompilation is a natural extension of traditional reversing. Reversers already generate a code-like representation of the programs they work on, so turning reversing into decompilation is simply a matter of writing this representation in a well-defined language—and recovering a precise representation of an entire function. Perfect decompilation is no longer purely an extension of the existing process, as it requires more effort to convince the compiler to generate specific output, but we found that even in this case the process correlates well with function comprehension.

### Discussion
One of the major highlights that stood out from the paper was the large amount of data collected by the authors. Everyone accepted and really appreciated the amount of data that the authors procured.

One of the patterns that we found from this paper is that conferences are more likely to accept papers that reach a similar conclusion as prior work. And this paper does verify the results of prior works. However, some of the reviewers felt like there wasn’t much else in the paper in terms of a previously unknown scientific knowledge. The huge data set that the authors made available definitely helps further research in this direction.

One of the members of our class had participated in the experiment conducted by the authors of the paper, and this person brought up the issue of having to “fight the compiler” to achieve the exact similar assembly code which many of the participants had to do during the experiment. An alternative way of comparing the compiler’s Intermediate Representation was suggested so that participants could avoid this problem. 

Another research question that could’ve been tackled was to identify if an automated approach can learn from the huge data set collected by the authors to create perfect decompiled code.

One of the more important questions that was raised during the discussion was about the relevance of perfectly decompiled code and whether that helps reverse engineers in any way. And the answer to that, is no. However, perfectly decompiled code can be used for instrumenting which can then be used for low overhead fuzzing of even black-box applications.

In the reviews, all of the reviewers had marked the Overall Merit of the paper as “Accept” and were very happy with the paper except for some minor issues.


### Conclusion

This paper proposes a new metric to quantitatively evaluate reversers' understanding of binary programs. The metric it uses is the distance between the recompiled binaries and the original binaries. By performing a large-scale online competition based on the metric, it demonstrates human experts are capable of achieving perfect decompilation. Although the paper has minor flaws (e.g. perfect decompilation vs reverse engineering), its objective measurement also gives the community more insights into how human experts carry out reversing tasks.

