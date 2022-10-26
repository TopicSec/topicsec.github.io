---
title: "In-Kernel Control-Flow Integrity on Commodity OSes using ARM Pointer Authentication"
author: Team Hot Source Rides Again Again
categories: [ cryptography, operating systems ]
tags: [ cryptography, operating systems. control flow, kernel]
pin: true
---


## Paper Info
- **Paper Name**: In-Kernel Control-Flow Integrity on Commodity OSes using ARM Pointer Authentication
- **Conference**: USENIX '22
- **Author List**: Sungbae Yoo, Jinbum Park, Seolheui Kim, Yeji Kim, and Taesoo Kim,
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-yoo.pdf)
- **Food**: No food

# Paper Summary
Introduction:
The foremost problems in operating systems are memory safety issues. Modern mitigation techniques as KASLR, DEP, SMA/EP and return and jump oriented programming are developed and deployed. Control flow integrity enforces the program to follow control graph at the compilation time which helped in mitigation of the exploitation techniques.

There are various developments happening in CFI design space to enhance the existing CFI’s and making CFI protection faster and practical. Apple recently tried to speed up CFI by utilising hardware based protection called ARM Pointer Authentication. This paper proposed another CFI protection kernel called PAL, this enhances the CFI precision while imposing the negligible performance overhead. As part of this design the context analyser captures common idioms and design patterns to enhance the precision.

PAL also has a static validator which recognises the common pitfalls and attack version on the kernel image. Kernel image meets all the necessary requirements if the static validator assures it has no pitfalls.
PAL exhibited better CFI precision compared to Android’s CFI. PAL is the first PA scheme which evaluated performance on Mac mini based on M1 chip.

Background:

Control Flow Integrity:

Code reuse attacks are the most common attacks, an attacker performs arbitary computation by chaining existing code gadgets after hijacking the program control flow. To avoid these attacks the integrity of control flow transfers should remain intact which is called Control Flow Integrity(CFI). CFI schemes mainly rely of the forward transitions, considering the backward transitions are handled by other techniques like shadow stack. PAL uses both forward and backward transitions to design the solution.

Pointer Authentication:

Pointer Authentication is a hardware security feature with minimal performance. A pointer is signed when the code is generated and is authenticated before its use. If the authentication succeeds the restored function or data pointer is used. If the authentication fails, it flips the error bit in the pointer and any later use of the pointer will raise exceptions.

Overview:

Threat Model:

Threat model is created as assuming the attacker has capabilities of arbitrary reads and writes at arbitrary moments and the victim has all modern defence mechanisms like secure boot, DEP, KASLR, PXN. Also assuming the KASLR can be bypassed by inferring the layout of kernel image.

Attack Vectors:

Replaying Attack: A leaked signed function pointer can be reused by the adversary read capabilities and scan the entire kernel memory and collect all the function pointers.

Forging pointers via signing gadgets: A adversary can create a signed pointer with the chosen context for indirect call.

Brute forcing attack: PA uses bits to embed MAC as part of pointer, but this can be easily identified by enumerating the input space.

# Design
The PAL has 3 parts: context analyzer, compiler tool-chain and static validator.
The PLA compiler signs a function pointer at its generation and authenticates it right before use. Since the PA-based solution is largely dependent on the proper selection of a PA context. The context analyzer of PAL spots adequate places for adopting run-time contexts with the given kernel code and the desired precision level. The static validator is used to evaluate PA-based solutions.
# Implementation
The PAL implementation is based on GCC 7.4.0 with new GIMPLE pass as plugin. The authors modified Linux and FreeBsd kernel to let the context analyzer automatically add annotations, prevent preemption hijacking and protect the context the backward-edge.
# Evaluation
To evaluate the accuracy of protection, the author compared PAL with other protection schemes such as Android scheme, iOS scheme and PARTS. The evaluation metric is the number of indirect calls.The results show that PAL is more accurate than other solutions.
Also, the PAL proved that it can prevent exploits of 3 known CVEs and refines the Linux and FreeBSD context.

# Review Summary



# Discussion Summary

