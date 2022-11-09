---
title: "Ground Truth for Binary Disassembly is Not Easy"
author: Team Hawaii
categories: [ disassembly, cfg, binary analysis ]
tags: [ disassembly, cfg]
pin: true
---


## Paper Info
- **Paper Name**: Ground Truth for Binary Disassembly is Not Easy
- **Conference**: USENIX '22
- **Author List**:  Chengbin Pang, Tiantai Zhang, Ruotong Yu, Bing Mao, Jun Xu
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-pang-chengbin.pdf)
- **Food**: Starvation

# Paper Summary
Stuff

# Discussion
The discussion on this paper was very positive. As has been the trend for papers from `Pang`, the authors did a great job a both motivation and explination of their paper. In addition, the tools developed to make this paper's eval was [open-sourced](https://github.com/junxzm1990/x86-sok). With this in mind, we thought the paper was great; however, there were still places in this paper that could use improvement.

In section 4 of this paper, the authors attempt to motivate why having ground-truth disassembly is important. We felt the "Impacts on Tool Evaluation" felt week at times. The comparison of state-of-the-art tools that use CFG's with and without OracleGT had some logical holes. In this comparison, it showed that tools without OracleGT preform worse when compared to the human verified CFG. It's hard for us to tell from a glance that OracleGT always has better CFGs than human analysis, which makes the claim that tools performing worse is because of an innacurate CFG used in their eval. We felt this section could've been tighter with an analysis of how their CFG outpreforms human made CFGs. 

In section 6 of this paper, the authors explain the approach and methods for making OracleGT. This section was concise, but we felt some important information about the implementation of this approach was missing. OracleGT is more accurate than a normal CFG because it instruments GCC on the intermediate language level, known as RTL. Some of our more technical reviewers felt that only instrumenting the RTL level of GCC was inssuficient and that IPA (another pass of GCC) should be implemented. For shortness, the explination was that IPA has a significant effect on the CFG and the authors should've either implemented or mentioned it in the drawbacks section. 

Finally, throughout the paper a large amount of information was presented via specific exampled or case studeis (like section 5). We felt that the large-scale evaluation was a little unclear. We were unsure how the authors selected the targets they did or why in some cases OracleGT did worse. We felt a unifying story for these sections was missing. 

In conclusion, we though this paper presented a promising approach that will certainly help the field of binary analysis. All our reviews accepted this paper. Though with it's flaws, we still felt this paper pushed the field forward.