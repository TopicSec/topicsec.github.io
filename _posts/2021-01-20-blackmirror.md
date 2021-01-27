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

Cheating in online games is a problem that has plagued online gaming since its inception, and in the past few years has turned into an all out arms race between game and cheat developers. Veteran online gamers might remember early anticheat systems such as PunkBuster, Valve Anti-Cheat (VAC), and Blizzard's Warden being put to use in the early 2000s. However, recently anti-cheat has been rapidly evolving as cheats become harder and harder to detect and combat. BlackMirror proposes one such solution for combating a certain type of cheats: wallhacks. For the uninformed, wallhacking is the ability in a multiplayer game to see other players through opaque objects such as walls, giving them a large advantage over other players. Think of it like playing hide and seek, but you can see where everyone is through obstacles. 

![](/assets/img/2021-01-20-blackmirror/wallhack.jpg)

To understand a solution to wallhacking, one must first understand how they work. Modern multiplayer online games operate with a client-server model where multiple clients are connected to a remote dedicated gameserver. They each communicate with eachother to maintain their own copy of a game state, which is essentially state-based knowledge of where players are, what they're doing, and when they're doing them. This game state is then used to calculate exactly what the player sees each frame on their monitor, where a typical rendering pipeline is used. This involves checking which enemy players are visible by checking their world coordinates against the player's world coordinates by casting a ray from the player to the enemy and checking for obstacle collision, which would indicate the enemy is behind an object and therefore occluded. Wallhacking involves disabling this check, and displaying enemy players regardless of whether they're obstructed by an obstacle. Many wallhack cheats will also highlight enemy players with vibrant colors to make them even more visible.

Note that the above is possible because the game state on the client of a player contains *all* of the information about the game at hand, so even if an enemy player is obstructed, the player's local game state on their client still contains their world coordinates, so they can be drawn if the check is disabled. The logical conclusion would be to perform checks on the server to only give the player a partial game state, where only visible enemy players are included, but the expensive nature of the calculations required means that it's not viable to perform these checks on the game's server.



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
