---
title: "Censored Planet: An Internet-wide, Longitudinal Censorship Observatory"
author: Team (p)Mango
categories: [Internet, Censorship]
tags: [Internet, Censorship, Measurement]
pin: true
---

## Paper Info
- **Paper Name**: Censored Planet: An Internet-wide, Longitudinal Censorship Observatory
- **Conference**: CCS '20
- **Author List**: Ram Sundara Raman, Prerana Shenoy, Katharina Kohls, Roya Ensafi
- **Link to Paper**: [here](https://dl.acm.org/doi/pdf/10.1145/3372297.3417883)
- **Food**: Fruit salad

## Problem

## Solution

Censored Planet is a platform designed to measure internet censorship across six protocols (IP, DNS, HTTP, HTTPS, Echo, and Discard) using four extant techniques:
 - Augur uses IP packet IDs as a side channel to detect TCP/IP blocking without requiring the active assistance of the machine whose connectivity is being measured.
 - Satellite and Iris detect DNS manipulation comparing trusted resolvers to the results from open resolvers.
 - Quack uses public Echo/Discard servers to detect keyword censorship and the directionality of blocking.
 - Hyperquack extends the same techniques from Quack to HTTP(S) by inserting keywords into HTTP headers sent to web servers with known behavior.

For the actual design of Censored Planet, refer to figure 1 in the paper as well as section 3.

Naturally, there are ethical considerations to this sort of project.
Most censorship measurements rely on hosts inside censored countries to intentionally attempt to trigger censorship, opening them up to potential retaliation.
Censored Planet's measurement techniques work at a distance from end users (2 traceroute hops for Augur), minimizing their risk.
Additionaly, the overhead incurred by sites used to measure against is considered with rate limiting in place as well as open WHOIS data and an informative page served on port 80 of participating hosts with the option to opt out.


## Discussion

The paper itself points out that their approach has inherent limitations such as course granularity of vantage points, as well as limitations of the measurement techniques they built upon.
Additionally, their measurement probes are very clear about their purpose and could conceivably be blocked or intentionally misled by censors.

Our discussion of the paper mainly focused on the human rights impacts of censorship and the importance of these sorts of measurements as well as personal experiences with internet censorship.
We also discussed the sheer magnitude of computing power required to analyze all traffic across a country for the purposes of censorship.
Another topic that came up was the prevalence of this sort of censorship paper, as most of us hadn't come across this sort of thing before which is also a testament to its quality since it was still approachable.
A notable exception to that was the charts, particularly the one on the coefficient of variation by country which remains impenetrable for this author.
