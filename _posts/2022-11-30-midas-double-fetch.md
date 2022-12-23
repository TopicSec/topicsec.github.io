---
title: "Midas: Systematic Kernel TOCTTOU Protection"
author: Team Not Yan
categories: [ Linux-Kernel, defense ]
tags: [ kernel, memory allocation, exploit]
pin: true
---


## Paper Info
- **Paper Name**: Midas: Systematic Kernel TOCTTOU Protection
- **Conference**: USENIX '22
- **Author List**: Atri Bhattacharyya, EPFL; Uros Tesic, Nvidia; Mathias Payer, EPFL
- **Link to Paper**: [here](https://www.usenix.org/conference/usenixsecurity22/presentation/bhattacharyya)
- **Source Code**: https://hexhive.epfl.ch/midas
- **Food**: Coffee

# Background

Page tables are sets of virtual addresses allocated to each separate executing process. These addresses are mapped onto the physical memory addresses by the operating system. Translation of virtual addresses to real addresses entails a performance overhead, but ensures program independence and facilitates caching techniques and swap files that give programs the illusion of nearly unlimited memory when necessary. 

Memory protection is the isolation of critical kernel-based code from less trusted user code. Kernel-level code has system and hardware access. User-level code is given no access. User-level code must make system calls to send requests to kernel-level code to access hardware. Kernel-level code must be isolated from possibly dangerous data within user-level code. One technique is through the architecture of supervisor memory protection, the requirement of using transfer functions to move data securely from user to kernel memory before utilizing it in any way.

# Paper Summary

Double fetch bugs exist in all major operating systems and bypass traditional supervisor memory protection.  If concurrent data modification is allowed in user memory, it is possible to modify data pulled into the kernel from user space during a system call by using a second thread. An example of an attack utilizing a double fetch bug would be a user-space thread triggering a system call to execute kernel-level code. The kernel code would read the length of a buffer from user space and allocate a kernel buffer to copy the data into. Concurrently the 2nd user-level thread would overwrite the length of the buffer in user-level memory before the kernel performs a copy procedure. The modified data would be longer than the expected length and overflow the kernel buffer.

Simply locking memory and putting all threads attempting to access memory to sleep may create dependencies between tasks and lead to deadlock. An alternative is to create multiple copies of the page for the user-level tasks to modify during the system call. Midas addresses this by locking memory or by taking a snapshot of memory pages accessed by system calls, preventing them from being changed during the life of the system call. The system call performing subsequent reads to the same memory pages will always return the same values. This is implemented by only minimal additional code. This protection results in only a minimal performance and resource penalty.

Midas does this by implementing a type of multi-versioning system. With existing memory handling functions of the operating system, namely page tables and copy_from or copy_to user functions, Midas allocates a second memory page for user space functions to modify on demand during the life of the system call, while keeping the original page unchanged for the system call to access.  When the system call finishes, the modified page is copied to the original page, and the allocation of the modified page is no longer needed and is released.

![](/assets/img/2022-11-30-midas-double-fetch/graph1.jpg)

This diagram demonstrates the implementation of Midas' multi-versioning system for memory pages. All pages are initially unprotected and unduplicated. When a kernel-level memory call is detected, such as copy-from-user or get-user, the page is marked in a protected/unduplicated state. If no other user-level thread attempts to write to this memory page before the end of the system call, the protection is released and causes no additional overhead.

If a write occurs during the system call, a duplicate of the page is made. The read-only page is utilized for kernel reads, and the R/W version is made available for all accesses by user-level threads.  If a second system call comes in before the first system call is finished, the R/W page is locked as read-only for the 2nd system call, and any additional writes would make extra duplicate pages.  As system calls come to an end, their locked pages are unlocked and copies are combined back to the original page.

There are some exemptions to this model. Futex is implemented by system calls performing essentially a double fetch style call by design.  These calls need to be added to an exemption list for Midas.  Another possible conflict is caused by protection being handled at the page level: if 2 objects are on the same page (O1 and O2), O1 is accessed by the system call, and O2 is written to by a user-level thread simultaneously.  This triggers the multi-versioning system inappropriately and wastes a slight amount of resources, even though no actual conflict exists.  

Midas has the benefit of preventing deadlock situations triggered by paged memory resources. Midas can also be used for bug hunting. With slight modification, it can detect and log when double fetch situations occur.  Unfortunately, a large number of false positive flagging due to scenarios (similar to the case of 2 resources within a single page) prevents this from being too useful.

# Discussion

The primary value of this innovative prototype is to demonstrate that a real-world double fetch protection implementation is feasible and would not require a significant memory or performance overhead. Double fetch bugs are a significant security risk since they can cause exploits up to full system compromise within kernel space. In theory, maintaining consistent values of user data during system calls should completely mitigate all double fetch bugs. All previous related works rely on either static or dynamic analysis that cannot completely resolve all possible double fetch bugs.

It would've been helpful to get a better sense of how prevalent double fetch bugs are in the real world, in terms of how common they are in comparison to other types of exploits, and how frequently their exploits are severe.

The evaluation of this paper, demonstrated against only one double fetch sample, could have been a bit more complete. This is not a strong enough evaluation to confidently state that Midas is capable of mitigating all possible variations of double fetch attacks. The theory appears sound, but this could have been demonstrated either quantitatively or qualitatively.

A qualitative approach could have been a systematic greater exploration of all of the theoretical variations of possible double fetch attacks. A quantitative approach would be to plug in an evaluation set composed of a large number of real-world double fetch attacks. This not only would more effectively demonstrate that Midas was useful against all variations of attacks, but also  would test the claim of requiring minimal overhead in all cases, and demonstrate that Midas does not cause compatibility problems when executing a large range of user-level code.
