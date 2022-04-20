---
title: "SmashEx: Smashing SGX Enclaves Using Exceptions"
author: Team paper cuts
categories: [ secure-enclave ]
tags: [ Intel-processors, vulnerability-research ]
pin: true
---

## Paper Info
- **Paper Name**: SmashEx: Smashing SGX Enclaves Using Exceptions
- **Conference**: CCS '21
- **Author List**: Jinhua Cui, Jason Zhijingcheng Yu, Shweta Shinde, Prateek Saxena, Zhiping Cai
- **Link to Paper**: [here](https://dl.acm.org/doi/10.1145/3460120.3484821)
- **Food**: Cherry Pie

# Background

The SGX enclave is a special hardware isolated computational environment that can handle secure data through isolation and encryption, even with an un-trusted operating system. This extra layer of protection has many uses, primarily providing an extra layer of security for critical security activities such as handling public and private key pairs.  Secure enclaves can run secure code in a scenario such as Amazon cloud - even if you do not trust the Amazon cloud operating system, you can still run code securely and be confident that even the operating system should have no way to breach the confidentiality of the data being executed.

A SGX enclave utilizes a host process to transfer control from the operating system into the enclave region, and the enclave has access to encrypted virtually mapped memory that is allocated to it alone. The enclave only trusts itself, its own private area, its own private stack, and hardware. Everything else is considered un-trusted.  The operating system maintains full write permissions for all memory addresses, but as the operating system cannot decrypt the enclave private memory, any attempt to write to enclave memory should scramble data and result in the application crashing and nothing else. Because the enclave software is executed in user space, all exceptions and system calls require exiting and reentering the secure area since the operating system is required to handle all kernel space functions.

# Exception Handling

Modern operating systems utilize synchronous and asynchronous system calls.  Synchronous calls are operations triggered intentionally by the software to request higher level privileges, and asynchronous calls represent interruptions of the code that the software did not expect or control. Examples of these include divide by 0 scenarios, hardware interrupts, or other resource or permission constraints that the operative system must handle.

In order to make ocalls (outside calls) the enclave must perform the following steps: save the system state to protected memory, scrub the system state, transfer control flow to the host process with a eexit, return to a fixed point with a eenter, and then restore the CPU state.  

Asynchronous system calls during enclave execution have additional transitions between protected and unprotected areas. Instead of a single exit and enter, the exception AEX (asynchronous enclave exit) returns to the host process, which then returns to the enclave to run the specific exception handling code, returns to the host process outside the enclave again, and then resumes the operation of the code within the enclave.

# Threat Model

The goal of this particular attack is to disclose the victim's memory contents or execute arbitrary code within the secure enclave through an exploit utilizing return oriented programming.  The key vulnerability being exploited this paper is that these operations (AEX, EENTER, EEXIT, ERESUME) are not atomic, they can be interrupted themselves in the middle of saving or restoring CPU registers.  ALSR by default is  disabled when executing SGX enclave software.  An asynchronous exception is hit in the middle of the re-entrance process, leading to an arbitrary write primitive.

# Preliminary Reviews

|             | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ----------- | ------ | ------ | ------ | ------ | ------ | ------ |
| Review #11A | 5/5    | 2/4    | 4/5    | 2/3    | 4/5    | 2      |
| Review #11B | 5/5    | 2/4    | 4/5    | 2/3    | 4/5    | 1      |
| Review #11C | 5/5    | 2/4    | 4/5    | 2/3    | 4/5    | 1      |

![](/assets/img/2022-04-20-smashex-smashing-sgx-enclaves-using-exceptions/vuln.png)

# The Attack

At step #2, we are within the un-trusted region and can change arbitrary values for any or all of the system registers, such as introducing a poisoned stack pointer. A page fault is triggered by denied permissions, or a timer interruption at step #4. This prevents the poisoned registry values from being cleared. At step #5 those poisoned values are moved into the secure stack allocation. At step #6 the system moves the poisoned data into the active stack running in the secure enclave, overwriting the return address of the ocall handler.  Since the exception handler and the enclave code both are within the secure region, the registers remain poisoned even after the exception is complete and the regular execution resumes in steps #8 and #9.  This return address is then called in step #9 giving the attacker control of program execution and allowing the program to be diverted to an arbitrary address such is the beginning of a ROP chain.  

ROP chains then have the ability to bypass security checks for enclave operations and perform operations such as copying sections of memory from the secure region into the unsecure region, or copying shell code from the insecure region into secure region.

Several approaches could limit the effectiveness or mitigate these types of attacks.  Temporarily disabling interrupts and exceptions by making requests to the operating system for permission to disable them could enforce atomic operations and prevent these attacks and taking place. Another approach would be to only allow ERESUME and not EENTER during enclave events.

# Discussion

This particular paper is very significant because it demonstrates for the first time an attack on SGX that requires no existing vulnerabilities in the software itself. The only limiting factor is the fact that SGX is being phased out by Intel and being replaced by by Intel TME.  This and similar papers really showcase the potential vulnerabilities of context switches when combined with concurrency issues in a universal way, not necessarily tied only to issues with secure enclaves. Similar transitions in and out of secure areas when combined with concurrency looks to be an interesting area of potential research.

The defense and limitations sections of the paper are well-developed, as well as special implementations such as Google's SGX, Google Asylo.  Diagrams and figures are very well-designed with an excellent color scheme. Semantically the layout of the diagrams presented all the important information necessary to understand the diagram, of course, a few additional labels could have been used to identify sections and show the temporal nature of the transitions for example.

More detail about the properties and behavior of SGX would have been appreciated for those without a strong SGX background. The sparseness of these details in this paper is understandable since the target audience of the paper are SGX experts, especially since the paper is already fairly information dense. Further implementation details (for example the technique used by the authors to change registers on the host operating system) would have been also useful. Of course the authors to provide a request form to allow those interested to obtain explicit attack examples.

