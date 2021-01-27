---
title: "BlackMirror: Preventing Wallhacks in 3D Online FPS Games"
author: Team Pappelmouse
categories: [Secure Enclaves]
tags: [secure, enclaves, gaming, anticheat]
pin: true
---

## Paper Info
- **Paper Name**: BlackMirror: Preventing Wallhacks in 3D Online FPS Games
- **Conference** CCS '20
- **Author List**: Seonghyun Park, Adil Ahmad, Byoungyoung Lee
- **Link to Paper**: [here](https://lifeasageek.github.io/papers/seonghyun-blackmirror.pdf)
- **Food**: Pomegranate

## Problem


## Solution


## Discussion

This paper did a really good job in explaining the basics of game architecture, describing the entire pipeline on how entities ultimately get rendered on the GPU.
Conceptually, it makes sense that with a secure computing enclave that securing sensitive entities could be possible.
However, it would have been nice to include more background information on how these secure computing enclaves work.
Getting a good basic understanding of IntelSGX would have gone a long way in clearing up the "magic" of the enclave.

This paper sparked a discussion on what anti-cheat gaming might look like in the future.
This approach of course relies on CPUs having secure computing enclave features available (IntelSGX was introduced in 2015), which may take a long time, leading us to believe this idea is likely a decade or more out from being part of the gaming industry.
Further, as mentioned in the paper, this idea does not seem to scale out towards other prevelant cheats like aimbots.
We wonder what other approaches are being investigated for anti-cheat, and suspect that ML may be relevant.
Further, we wonder what role cloud gaming, like Stadia, might play in the future of anti-cheat.
After all, remote servers are inherently a much more secure computing enclave than a local machine ever can be.

![](/assets/img/2021-01-20-blackmirror/stadia.jpeg)

The biggest take away from this paper, though, is that gamers are hardcore.
