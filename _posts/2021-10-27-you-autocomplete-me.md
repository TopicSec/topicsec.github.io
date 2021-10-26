---
title: "You Autocomplete Me: Poisoning Vulnerabilities in Neural Code Completion"
author: Team Hot Source
categories: [machine learning]
tags: [machine learning, poisoning, code completion]
pin: true
---

## Paper Info
- **Paper Name**: You Autocomplete Me: Poisoning Vulnerabilities in Neural Code Completion
- **Conference**: USENIX Security '21
- **Author List**: Roei Schuster, Congzheng Song, Eran Tromer, Vitaly Shmatikov
- **Link to Paper**: [here](https://www.usenix.org/conference/usenixsecurity21/presentation/schuster)
- **Food**: No food

## Summary

## Discussion

### Accolades

### Attack Vector

### Novelty

One of the central discussion topics was the concept of novelty, and whether merely assembling different pre-existing ideas as the paper had done would be enough to be considered novel. Novelty is a commonly bandied-about word in academia when discussing the quality of research papers, with more novel topics usually being regarded as 'better' (one member of the discussion group even stated that the distinguished paper award should only be given to novel papers). However there seem to be varying standards on what one considers novel. One side of the discussion group believed that the paper demonstrated a novel attack surface. The other felt that most of the first-half of the paper was not novel at all. Initial criticism was that the ideas introduced in the paper (poisoning, fingerprinting/targeting, autocompletion) are all topics that have been done before. To quote: "Does this paper advance science?"

The two camps that emerged can be broadly classified as:

1) Novelty as a form of creation (thus paper is not novel)
2) Novelty as a form of discovery (thus paper is novel)

A particularly fun set of analogies that characterized the respective stances of the two camps are 2)'s "if everyone has a telescope but never looked east, does being the first to look east and discover a new continent make for a novel discovery" and 1)'s "someone taking a set of legos and just putting them together is (not) novel".

In 1) a brand new idea or technique is considered novel. A paper on a new attack is novel. Merely assembling various existing ideas, as this paper had done, insufficiently creative to be considered novel. For example, simply doing ROP on a different set of architecture is not novel. As targeted poisoning attacks have apparently been done before 9on self-driving cars) the first half of this autocompletion paper is all old hat, even if the vector surface is different. Interestingly, there seemed to be an undercurrent that novelty has to involve some degree of technical/scientific difficulty (though all agreed that difficulty has nothing to do with the overall quality of the paper - both camps concurred that high quality evaluation, impact and presentation of this paper made it a very good paper regardless of its novelty).

For 2) a discovery is considered novel. Measurement papers are novel. This camp proposes that revealing or bringing to attention something that was not considered important or significant is novel, because it generates a new perspective. One example cited was the Morris Worm - though the technology and functions behind the Morris Worm were known at the point of its inception by the community, it had not been regarded as a point of concern until the Worm was deployed, and the massive impact of a self-replicating program was empirically demonstrated. This shift in perspective is novel, and worthy of being recognized. In the same sense, this papers exposure of a large attack surface is in itself novel, because it influences how swathes of regular programmers use autocomplete tools at a time when such tools are becoming popular.

Ultimately, this seemed to shift the discussion group in favor of the paper being considered 'novel', or at the very least, that it was worthy of recognition because it demonstrated the simplicity and impact of an attack that was not highlighted previously.