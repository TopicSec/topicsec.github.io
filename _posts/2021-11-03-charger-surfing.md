---
title: "Charger-Surfing: Exploiting a Power Line Side-Channel for Smartphone Information Leakage"
author: Team Apollo
categories: [Side-Channel]
tags: [Side-Channel, info-leak, smartphone]
pin: true
---

## Paper Info
- **Paper Name**: Charger-Surfing: Exploiting a Power Line Side-Channel for Smartphone Information Leakage
- **Conference**: USENIX Security '21
- **Author List**: Patrick Cronin, Xing Gao, and Chengmo Yang
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec21-cronin.pdf)
- **Food**: No food

## Prerequisites
Side-channel attacks are a common attack method in which information is gained by measuring and analyzing resultant patterns usually external to the immediate process at hand. An example of this could be a timing attack in which a password is cracked by utilizing how long a string comparison takes, or listening to the sounds of a user typing to determine what they're typing.

## Problem
In this paper there isn't a distinct problem the author's are targeting, instead, they showcase a very interesting method to glean information about screen content and button presses by analyzing the smartphone's power trace when it is plugged in and charging.
This method allows attackers to precisely identify the location of virtual button presses on the touchscreen, meaning they can use it to steal sensitive information.

## Threat Model
The threat model in this paper is a realistic public scenario where users charge their smartphones either with a cable provided for them, or a USB outlet to plug a USB charger into.
The attacker would control one of these devices, and would monitor the power consumption of the connected device hidden behind the charging interface. This would be done with a low power microcontroller that could record or stream the traces.
The attacker would also have no previous knowledge of the victim, would not know what kind of smartphone the victim would have, and would have not measured their specific device in the past.

## Evaluation
The authors collected two datasets, one broad set (4 different phones, 15 different users), and one detailed set (two devices, 33 users). They collected the data with a setup claimed to be under $20 for a microcontroller and an analog to digital converter.
In the broad dataset, they varied the number of training users and had an average of 98.7% accuracy with 5 training users, and an average of 50.1% with 1 training user.
They note that they also discovered that sampling frequency also determines the practicality of the attack, where sampling below 31.3KHz results in accuracy loss.
With the detailed set, they tested cross device with an iPhone 6+ and iPhone 8+, where they would train on one and test on the other, resulting in a minimal difference from the broad set. 
They also tested if phone conditions would alter the results, with varying brightnesses, charge levels, wallpapers, and also haptic feedback, which all resulted in minimal accuracy impact.

## Discussion
You're in for some fun stuff since this paper generated quite a bit of discussion!

### "Spy-stuff"
The conversation began with a discussion about the paper's attack as being cool spy stuff.
It's like something you'd see in spy movies, where you swap chargers, attack, and leak information! Such an attack involves a great deal of effort, which would only be worth it on high-profile targets.


### Evaluation

When it comes to machine learning, the true accuracy of a model is measured by its performance on new datasets that were not included in its training set.
The authors did their evaluation where they train and test on different phones and users.
They use Phone A and users 1 and 2 for training their model. 
And test the accuracy on Phone B, which was used by 8-9 other users.

![](/assets/img/2021-11-03-charger-surfing/1.JPG)

Then, the discussion shifted to how often passcode authentication is used nowadays when we have face ids and biometrics.
We discussed different scenarios, where the user could choose to use passcode authentication or could be forced to use it.
Using passcode could be easier for the older generation and they could prefer it over a biometric.
When everyone was wearing masks during Covid, face authentication did not work and the user was forced to use a passcode. 
In a recent iOS update, users can now authenticate with their iWatch.
Another scenario is when you switch on the phone, you have to enter the passcode, you can’t use a face id to authenticate yourself. 
In addition, if you repeatedly press the power button, the user will be forced to enter a passcode.
Or if the user has a super old phone with only passcode authentication.
Above were a few scenarios, which came out where the user would likely use passcode authentication.

There was an interesting question, that this paper seems like a cool hack that could make a nice DEF CON talk. 
What is that element or aspect that elevates this cool hack into an academic paper?
Maybe because they demonstrated a new kind of attack (there is no clear answer).

And this kicked off our classic discussion about novelty!

### Novelty
[The Cambridge Dictionary](https://dictionary.cambridge.org/us/dictionary/english/novelty) defines novelty as _" a quality of being different and unusual"_.
The concept of novelty can be very subjective and mean different things.
The same paper or technique can be considered novel in cybersecurity, but not in the field of program analysis.
When one puts together two pieces from different domains and then finds a connecting bridge that results in improvements. This may be regarded as novel (or creative) in one field, but not in another.

#### _Mantra for novelty_
- Come up with an awesome new technique 
- Apply the existing technique to fundamentally different and new datasets
- Scale up or speed up existing techniques by a significant amount

To return to the original question, would this paper be novel enough for a venue that is non-security, or would it be too security-specific for such a venue?

### Gaming
Next, we discussed how similar attacks could be applied elsewhere. 
Is it possible to use such attacks on gamers to find out what their opponents are up to?
The answer depends on the game. A phone game can have very basic graphics or it can be very complex.
Due to the intensity of the graphics, it would be too hard to filter out the noise. 
A further simplification of the attack is: Could you detect a difference in display power when the gamer switches from one screen to another?
If you could detect if a gamer clicked on the screen or if they opened a menu.
A bunch of cool and nice ideas were thrown around during the discussion. 


### Countermeasure
The authors do discuss a few countermeasures but may have been able to elaborate more on their application in the real world.
The group discussed the low-pass filter and randomizing the keyboard layout countermeasure as mentioned in the paper. 
Another interesting idea is to either disable animations or animate all the digits and the actual digit (the user presses) in a slightly different way than other digits.
By doing so, it could become difficult to detect the power drain. 

Similarly, the phone could be stopped from charging whenever the keypad comes up (from a software perspective).
On the hardware side, it should be possible to cut and reconnect, based on a full-handshake timing, or if the reconnect is software triggered.


### Next-steps
An initial step will be to determine whether these attacks are still effective on new phones.
For the study, the authors used the Moto G4, Galaxy Nexus, iPhone6+, and iPhone8+.

Another direction would be to try a similar attack with wireless charging.
There has been work done on recognizing patterns of wifi interference to find out where people are in the house. 
So, if something similar can be accomplished without the compromised wireless charger.
Alternatively, you could use a compromised wireless charger, and somehow induce a situation in which there is a cellular or wireless packet loss by changing the frequency a charger delivers to the device. 
In addition, it would be interesting to see some versions of these attacks from a firmware perspective.
