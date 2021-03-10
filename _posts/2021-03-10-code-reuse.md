---
title: "A Generic technique fro Automatically Finding Defense-Aware Code Reuse Attacks"
author: Team (p)Pomegranet
categories: [code reuse attacks]
tags: [code reuse attacks, software engineering]
pin: true
---

## Paper Info
- **Paper Name**: A Generic technique fro Automatically Finding Defense-Aware Code Reuse Attacks
- **Conference**: CCS '20
- **Author List**: Edward J. Schwartz, Cory F. Cohen,  Jeffrey S. Gennari, Stephanie M. Schwartz
- **Link to Paper**: [here](https://dl-acm-org.ezproxy1.lib.asu.edu/doi/pdf/10.1145/3372297.3417234)
- **Food**: cinanom roll

## Problem

Researchers have proposed automated systems that find code reuse attacks to address the difficulty for a defender to determine if a specific program would be protected any defense against reuse attacks. 

But many automated code reuse attack systems are not defense-aware. This means that they will completely fail in the presence such as CFI. There are just a few of these systems aware of CFI, but they are complex and only target a single, hard-coded CFI variant. 

Therefore, these systems are unsuitable for defenders trying to find a defense that protects their software adequately. 

## Solution

To address the proposed problem, this paper proposed a generic framework for automatically discovering defense-aware code reuse attacks in executables.

The proposed framework can produce attacks for defense by analyzing their runtime behavior.

This framework does not require hard-coded stragtegies or models of defenses-aware attack strategy has not yet been proposed.

The main idea of this method is that code reuse attacks can be reduced to a state reachability problem, and defenses naturally restric the state transitions that an attacker can employ.

The authors implemented the proposed framework in atool called LIMBO, and evaluated this tool in this paper.

Limbo reduces the problem of finding a code reuse attack to a SMC problem,which it solves using executablelevel concolic execution.

Concolic execution provides a balance between soundness, completeness, and scalability that is well suited for automating the search for code reuse attacks.

Concolic execution allows Limbo to take a state and enumerate some of the states that are reachable from it by executing the program symbolically. 

Limbo uses this ability to iteratively compute a set of states that are reachable from the vulnerability. 

As soon as this set contains a goal state, Limbo outputs the discovered attack as an executabletest case.Thetest case containsinputs that, when given to the program, will cause it to reach a goal state

## Evaluation

The authors evaluted how effectively Limbo can discover code reuse attacks using their state reachability approach by addressing the following specific research questions:

RQ1: Can limbo discover code reuse attacks in presence of fine grained cfi?

This paper puts the theory of being generic and ablilty to theoretically find defense-aware attacks without any special knowledge about a defense, to the test by evaluating whether Limbo can find find attacks in one of the most common defenses, Control Flow Integrity(CFI)

RQ2: How much executable code it requires to do a reuse  attack?

To answer this question, authors examine the relationship between the amount of code available for reuse and Limbo’s ability to find code reuse attacks.

RQ3: How sensitive is LIMBO to its heuristics?

To answer this question, authors evaluate Limbo’s performance while varying these modifications to understand their contribution to Limbo’s overall results.

RQ4: Can Limbo discover new techniques?

To answer this question, authors examine in detail an attack that Limbo found in given attacks. 


##Limitations

There are limitations of both our technique and implementation in this work. 

The most fundemental limitation of concilic execution is that it is a path-based analysisi, and most programs have too many paths to exaustively explore.

Limbo is limited to analyzing executables that its underlying concolic executor supports, which are 32-bit x86 Linux executables.

Another limitation of Limbo is that it does not find information leak vulnerabilities on its own.

## Discussion

This paper proposed a generic framework for automatically identifying defense-aware code reuse attacks which can produce attacks for multiple defenses by analyzing the runtime behavior of the defense. 

The authors implemented the proposed framework in a tool called Limbo by making a small number of modifications to an existing binary concolic executor, and evaluated Limbo, and demonstrated that it excels when there is little code available for reuse, making it complementary to existing technique. 

As a result this work showed that Limbo outperforms state-of-the-art tools for both automatically identifying DOP attacks in the presence of fine-grained CFI, and automatically producing ROP attacks.
