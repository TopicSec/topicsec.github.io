---
title: Security Mistakes Developers Make
author: Team Fable
date: 2020-09-09
categories: []
tags: [qualitative, mistakes]
pin: true
---

## Abstract
This paper aims to study and categorize the various types of mistakes developers make regarding security.
In order to do this, they leveraged a Build It, Break It, Fix It competition to obtain data on the introduction of errors and their subsequent fixes.
Vulnerabilities were categorized according to how they were introduced, level of control they allowed, and how easily they could be exploited.
Once the raw data was categorized, certain trends emerged: outright mistakes where the developer didn't implement what they meant to were relatively uncommon, while the most common type of vulnerability was that introduced through misconceptions of security concepts (API misuse, etc.).

# backround & data

based on a secure programming competition

- 3 phases, build/break/fix

 ## data

### project selection



### data gathered

- secure log
- secure communication
- multiuser database
- project characteristics

# research question

vuln types: conceptual mistakes

how much control gain

require to exploit

# methodology question

- representative of real world?

In Favor and Against



## categorize errors

no implement

misunderstand

mistakes

## quantify exploitable

discovery difficulties

- require execution

- require source code

- require insights on key algorithm

exploit difficulties





# results

## types of errors

that big table in paper

## risk factors of errors prevalence

- skip unintuitive

- more errors in popular language

- practices (code review, testcases) work

- no differences between professional and student
  - discussion of measuring sec profession 35 min
  - practical train/experience?
    - errors are all in course slides
    - they have 8 9 years dev experience
    - CTF?
    - team size/structure
    - so how to train and measure?



## risk factors of attacker control,

report by teams:

no implement: easy to find

misunderstand: hard

mistake: easy to find and exploitable



# recommendation

- match CWE/OWSAP

- hire
- better API design
- better API docs
- better education
- better automatic tools







# discussion:

in slides:

- how competition can measure this field more accurately

- pro vs student

- better static analysis tools





in video:

- dynamic/semantic ways:

hardcoded aes iv, just one type



- no tools on design/conceptual level

GPT3? (hardcoded rules?) a ccs nlp paper



- that unapproachable disassembly paper problem

follow up work
