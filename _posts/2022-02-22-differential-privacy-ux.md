---
title: "''I need a better description'': An Investigation Into User Expectations For Differential Privacy"
author: Team New Folder
categories: [ Security Communication ]
tags: [ differential privacy, human subjects ]
pin: true
---

## Paper Info
- **Paper Name**: ''I need a better description'': An Investigation Into User Expectations For Differential Privacy
- **Conference**: CCS '21
- **Author List**: Rachel Cummings, Gabriel Kaptchuk, Elissa M. Redmiles
- **Link to Paper**: [here](https://cpb-us-w2.wpmucdn.com/sites.gatech.edu/dist/c/679/files/2021/08/ccs21_dp.pdf)
- **Food**: Chocolate Chip Cookies

# What is Differential Privacy?

Differential privacy is a statistical technique designed to quantify and limit the amount of information that can be extracted about an individual in a dataset.
The technique works by measuring and reducing the _signal_ in collected data sets.
The goal of reducing the signal is to enhance respondent privacy while maintaining the statistical integrity of the survey, so that results still reflect the population, but the probability of an individual response being reflected is lower.

There are two kinds of differential privacy (DP henceforth): "local" and "central" DP.
Local DP is the gold standard - it obfuscates data during collection so that nobody can ever have access to the full, unobfuscated dataset.
Central DP is when the data is stored unobfuscated but obfuscation is added during presentation, so it is possible to see the unobfuscated data if for any reason you have access to the raw database.

# Paper Summary

Prior work has found that descriptions of user-facing privacy tools should be relevant, actionable, and understandable.
However, if you go out into the world and look at descriptions of DP that are provided whenever the technology is available, they are none of these.

This paper aims to guide future writing about differential privacy in order to accurately set user expectations by answering the following research questions:

- RQ1: Do users care about privacy in cases DP can help?
- RQ2: Are users more willing to share if they're confident disclosures will not occur?
- RQ3: How do descriptions of DP affect user expectations?
- RQ3: How do descriptions of DP affect user data sharing?

## The Surveys

The authors set up two Mechanical Turk surveys designed to answer the four research questions.
The core of the questions posed to respondents was, "Are you willing to share your sensitive financial or medical information in this setup?"

The setups being studied were six scenarios in which information could be disclosed, combined with three measures of the risk of the given disclosure.
The scenarios were six broadly differing descriptions of a different kind of party being able to access a user's sensitive data, for example, law enforcement obtains a warrant for the data, or a hacker could penetrate the responsible organization's security and exfiltrate the data.
The risks were specified as either "no risk", "the same as the risk of your bank account being hacked", or "greater than the risk of your bank account being hacked".

### Survey 1

The first survey, designed to answer RQ1 and RQ2, investigates the role of the risk of different kinds of information disclosure in users' sharing behavior.
It simply poses the scenarios, asks users which risks they care about, and collects their willingness to share their data.
It finds a fairly simple answer to RQ1, "yes".
Users do care about whether their information will be disclosed outside its intended usage, though through some scenarios more than others.
With regards to RQ2, the authors measured the increase in likelihood for users to share their data in the low and no risk scenarios vs the high risk scenarios, an "odds ratio" of `P(share | low risk) / P(share | high risk)`.
In all the situations where the result was statistically significant, users were in fact more willing to share their data in the low/no risk scenarios vs the high risk scenarios.

### Survey 2

The second survey was designed to answer RQ3 and RQ4.
This survey presents some additional information to the respondents: descriptions of differential privacy.
Users were shown at random either nothing (the control) or one of six descriptions of differential privacy, ranging from the completely unsubstantiated to the overly technical.
These descriptions were synthesized from a corpus of existing descriptions that had been found in-the-wild, and are thus believed to be a realistic representation of the way DP is described to users in the real world.
Users were then asked if they are willing to share their data, and whether they believe each of the privacy expectation scenarios from the first survey were true or false.
This produces two tables of odds ratios - one 6x6 and one 1x6.
The first table has one axis being the description of DP that was given to the user, and the other axis being the privacy expectation.
The odds ratios are the increase in likelihood that a user believed a given privacy expectation was true or not.
The second table is the increase in likelihood that a user is willing to share their data.
Both sets of odds ratios are the given explanation of DP versus without any explanation.

For RQ3, the data shows that different explanations of DP affect expectations with regards to different scenarios differently.
For RQ4, however, no explanation of DP significantly increased the likelihood of users sharing their data!
This seemingly contradicts the findings of survey 1, which indicates that if users have increased expectations of privacy, their data sharing will increase!

Additionally, there is the question of whether user expectations are set _correctly_, a sub-problem of RQ3.
Broadly, users in this study had expectations that were more closely aligned with the capabilities of central DP, i.e. they had lower expectations, however, the alignment was weak, i.e. users did not have expectations of DP that aligned with _either_ model.

## The Framework

To address this contradiction, the authors developed a novel framework for understanding the impact of descriptions.
Essentially, a description affects certain expectations, and users have certain concerns, and the more the concerns and the expectations overlap, the more the user's behavior will change.
This resolves the contradiction by noticing that not all users cared about all risks, and no description raised expectations related to all risks.
Therefore, it is totally likely that a given user would not have their concerns placated by the description they were given.

# Preliminary Reviews

|            | RevRec | RevExp | OveMer | RevInt | WriQua | EthCon |
| ---------  | --- | --- | --- | --- | --- | --- |
| Review #4A | 4/6 | 2/4 | 3/5 | 3/3 | 3/5 | 2 |
| Review #4B | 4/6 | 2/4 | 3/5 | 1/3 | 3/5 | 2 |
| Review #4C | 3/6 | 2/4 | 3/5 | 2/3 | 4/5 | 2 |

# Discussion

The discussion this week was very scattered.
The following points were brought up:

## Demographics

There was a lot of concern about the demographics of the users studied.
Notably, there was no speculation in the paper as to how demographics could have affected the results.
The authors cite that Mechanical Turk is representative of a certain subset of Americans, but non-American opinions and view are absolutely essential to this topic, since many of the companies deploying implementations of DP are global.
Furthermore, residents of different countries have different values and expectations related to privacy.

## Generalization of DP Frameworks

The split between local and central DP isn't as clear cut as the paper makes it out to seem.
For example, Uber is named as a user of central DP, but in the paper's introduction, the capability added by its DP (protecting users from data analysts) is typically considered to be a feature of local DP.
Should there be a third category highlighting this nuance?

## Weakness of the Framework

The "novel framework" presented toward the end of the paper seems insufficiently interesting to count as a scientific contribution.
However, we understand that the point of science is to produce a canonical record of measured fact, subjective as it may be in the case of human studies research, and noting and writing it down for posterity does have a use.

## Accessibility

It was noted that the use of colors to represent distinct entities in figure 3 was inaccessible to colorblind people.

## Perspective

Finally, we noted that this paper could not have been written a decade ago, when DP was new, even though nothing technical this paper relies on is newer than that.
Back then, DP was just a bunch of math, but today there are actual implementations of DP in the wild that have informed our cultural understanding of what it means for information to be protected.
