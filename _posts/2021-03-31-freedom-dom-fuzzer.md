---
title: "FREEDOM: Engineering a State-of-the-Art DOM Fuzzer"
author: Team Pappelmouse
categories: [Fuzzing, Web]
tags: [Browser, DOM, Grammar]
pin: true
---

## Paper Info
- **Paper Name**: FREEDOM: Engineering a State-of-the-Art DOM Fuzzer
- **Conference**: CCS '20
- **Author List**: Wen Xu, Soyeon Park, Taesoo Kim
- **Link to Paper**: [here](https://gts3.org/assets/papers/2020/xu:freedom.pdf)
- **Food**: Cookies

## Problem

Web browsers have lots of high-severity bugs.
The DOM (Document Object Model) engine, responsible for displaying interactive HTML documents, has extraordinary complexity, and is one of the largest bug sources in a web browser.

Smart fuzzing is the dominant approach for finding bugs within DOM engines, but are lacking for multiple reasons:

1. Static context-free grammars

HTML documents have rich semantics reflected by data dependencies within the document.
Existing DOM fuzzers fail to manage these dependencies, and as a result, suffer from semantic errors in their output documents.
This naturally results in a worse exploration of the state space.

2. Lacking intermediate representation

Existing DOM fuzzers output textual documents without preserving intermediate information.
As a result, the generated documents can only be mutated by appending new data rather than by many other operations such as data flipping and splicing.

3. Poor handling of program termination

Launched browser instances never automatically terminate unless a crash occurs.
It is difficult to know exactly when the input document is completed processed because of dynamic rendering tasks that may occur anytime.
As a result, existing DOM fuzzers set a timeout of 5-10 seconds.
This results in a severely low throughput.
Less throughput means less state exploration.

![](/assets/img/2021-03-31-freedom-dom-fuzzer/figure_1.png)

## Solution

## Discussion
