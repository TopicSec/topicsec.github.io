---
title: "CROSSLINE: Breaking ''Security-by-Crash'' based Memory Isolation in AMD SEV"
author: Team Paper-cut
categories: [ Secure Encrypted Virtualization, AMD processors]
tags: [AMD processors, Secure Encrypted Virtualization]
pin: true
---

## Paper Info
- **Paper Name**: CROSSLINE: Breaking ''Security-by-Crash'' based Memory Isolation in AMD SEV
- **Conference**: CCS '21
- **Author List**: Mengyuan Li, Yinqian Zhang, Zhiqiang Lin
- **Link to Paper**: [here](https://arxiv.org/pdf/2008.00146.pdf)
- **Food**: No food



# Paper summary: 

This paper investigates the security of the Secure Encrypted Virtualization (SEV) feature of AMD processors by presenting CrossLine attacks against AMD's SEV architecture, which nominally protects virtual machines against inspection by malicious hypervisors.  These attacks demonstrate a full compromise of the basic AMD SEV architecture and a partial compromise of confidentiality for SEV-ES. CrossLine attacks are stealthy; they do not require the target VM tobe executing, which makes the attack generally undetectable by the VM. CrossLine does not rely on a lack of integrity checking in encrypted data; rather, it exploits the window of time before a spoofed ASID crashes an attacking VM to impersonate a victim VM and leak encrypted data

The authors present two version of CrossLine attacks.  

- **CrossLine V1**: This version of CrossLine leaks page tables entirely and arbitrary memory subject to constraints on some bits. This attack is applicable to SEV-ES VMs.
- **CrossLine V2**: This version of CrossLine allows reads/writes to the encrypted memory of another process.

Both versions are expected to be mitigated by SEV-SNP, AMD’s next enhancement to SEV. 

# Proposed attacks: 
 
The attack works as follows (CrossLine V1):

- Run the victim VM to the point where it should be inspected and pause it
- Clone the ASID and nPT attributes (specifically the address space) from the victim VM to an attacker VM
- Manipulate the attacker VM's guest registers such that when it resumes, it will try to access an arbitrary memory address, then fault, and then begin a page table walk at a controlled location, the location we would like to decrypt
- If the unencrypted data in memory at the controlled location looks like a page table entry, the page table walk will continue one additional level and then fault while trying to access the memory pointed to by the controlled location
- This fault will be reported to the hypervisor, leaking the address pointed to by the page table entry 

CrossLine V2 uses V1's memory fingerprinting capabilities as well as the basic SEV's unencrypted state to selectively execute instructions from the guest address space in order to build encryption and decryption oracles by moving data into and out of encrypted memory.

# Preliminary Reviews: 

|              | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------  | --- | --- | --- | --- | --- | --- |
| Review #2A | 5/6 | 2/4 | 2/5 | 2/3 | 2/5 | 1 |
| Review #2B | 4/6 | 2/4 | 3/5 | 2/3 | 2/5 | 1 |
| Review #2C | 5/6 | 2/4 | 2/5 | 2/3 | 2/5 | 1 |

Only one reviewer considered themselves knowledgeable on this topic. Other reviewers considered themselves only to have either some familiarity or no familiarity. Two reviewers think that paper should be accepted as it is, and the other reviewer thinks the paper should be accepted with only minor revisions.  

There were ethical concerns, and all reviewers decided that submission appropriately mitigates potential risks or harm.

The paper presented an original, new type of attack well described with clear descriptions and technical details. They illustrate their contribution with a well-structured presentation of their approach.  However, most reviewers find the paper hard to read because of paper's terminology. 

# The Statistics: 

This paper provided a few useful statistics through several informative figures and tables. Their first table is very useful and clear, but subsequent sections of their paper have long sections of pure text that should have included more graphs or figures. For instance the background section and section 4.1 in particular could have used additional graphs to illustrate the complex technical details. Each “Performance Evaluation” subsection in sections 4 and 5 would have been better presented by additional tables.  This paper also lacked real-world case studies or attack demonstrations. 

# Motivation: 

The main purpose of this paper was to conduct an impactful examination of a novel security attack. Most reviewers agreed that the subject of utilizing a malicious hypervisor is an understudied topic.  There is a concern that with access to the hypervisor, most attackers would already have access to the whole system and additional exploits would have only limited real-world value. It is uncertain whether there will be any additional interest or research in this direction.

The paper clearly shows that SEV is vulnerable by demonstrating these novel attacks.  The paper also shows how changes to the AMD SEV architecture will mitigate these attacks.  Even though no short term modifications seem to have been made on AMD’s part to their existing architecture, AMD has made improvements to their future products as seen in the enhancements to SEV-SNP. Calling attention to the possibility of these types of attacks will undoubtedly make an impact in the long-term, leading to strengthened security implementations in the industry.

# Ethics consideration:

The authors performed responsible disclosure by contacting AMD prior to publication.  However, there is a concern that they should not release exact details/code of this exploit given that AMD does not intend to patch this bug in their immediate products.

