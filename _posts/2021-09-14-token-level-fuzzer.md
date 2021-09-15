---
title: "Token-Level Fuzzing"
author: Team bi0s     
categories: [Fuzzing]
tags: [Token Level fuzzer, Fuzzer]
pin: true
---

## Paper Info
- **Paper Name**: Token-Level Fuzzing
- **Conference**: USENIX Security '21
- **Author List**: Christopher Salls, UC Santa Barbara; Chani Jindal, Jake Corina, Christopher Kruegel and Giovanni Vigna
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec21-salls.pdf)
- **Food**: No food

## Prerequisites

### Fuzzing
Fuzzing is the generalized process of feeding random inputs to an executable program or an application in order to create a crash. Fuzzers can generate vast number of test cases to exercise the target application and monitor their run-time execution to discover security bugs. Most fuzzing research are categorized across three axes: Input generation, program access and coverage goals.

### Input generation
There are two main classes of approaches to generate inputs: Mutational fuzzing which modifies seeds of typically well-formed inputs to generate new inputs and generational fuzzing which generates input based on the structure and leverages description of the input format.

### Program access
There are typically two types of fuzzing approaches: White-Box Fuzzing and Black-Box Fuzzing. White-Box Fuzzing performs program analyses and collects constraints from conditional branches during run time which is then used to generate new inputs. On the other hand, Black-Box Fuzzing approach does not have any access to the internals and thus rely on certain heuristics.

### Coverage goals
Coverage based fuzzing is a technique which uses different types of tracking such as edge coverage, block coverage to track the input that maximize the code coverage which is later used to generate new inputs. 

## Problems

Modern day interpreters are very complex and require highly structured input composed of individual tokens.

This makes it difficult to fuzz interpreters using byte level fuzzing since randomly generating an input that contains proper tokens in a proper order has a very low probability. And if the input does not conform to the structure that the interpreter expects, it may throw an error and abort execution. Since the syntax checking occurs at a very early stage in the execution cycle of an interpreter, a majority of fuzzing cycles only exercise the input parsing stage of the interpreter and does not trigger other deeper segments thus limiting its capability to identify only shallow bugs.

In such a situation, a grammar based fuzzer can perform better in terms of generating syntactically correct input. Although, in order to do so, the grammar needs to be precise and cover all of the input language grammar.
Generating such a grammar is no easy task and combined with the fuzzer’s heavy dependence on the grammar, it becomes evident that retargeting a fuzzer for a new language is very difficult.

Additionally, if the fuzzer adheres too strictly to the grammar, it may not be able to find bugs that are triggered with a semantically or syntactically incorrect input. Recently, bugs have been discovered in real world applications like [V8](https://bugs.chromium.org/p/chromium/issues/detail?id=800032) and [Microsoft’s Edge browser](https://nvd.nist.gov/vuln/detail/CVE-2017-8729) that are only triggered when the input is not syntactically or semantically correct. The discovery of such bugs and their severity indicate that this class of bugs cannot be ignored in the context of fuzzing interpreters.

## Solutions

## Evaluation

## Discussion
