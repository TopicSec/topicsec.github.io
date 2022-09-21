---
title: "Fuzzware: Using Precise MMIO Modeling for Effective Firmware Fuzzing"
author: Hot Source Rides Again Again
pin: true
---

# Paper Info
- **Paper Name**: Fuzzware: Using Precise MMIO Modeling for Effective Firmware Fuzzing
- **Conference**: USENIX '22
- **Author List**: Tobias Scharnowski, Nils Bars, and Moritz Schloegel, Ruhr-Universität Bochum; Eric Gustafson, UC Santa Barbara; Marius Muench, Vrije Universiteit Amsterdam; Giovanni Vigna, UC Santa Barbara and VMware; Christopher Kruegel, UC Santa Barbara; Thorsten Holz and Ali Abbasi, Ruhr-Universität Bochum
- **Link to Paper**: [here](https://www.usenix.org/system/files/sec22-scharnowski.pdf)
- **Food**: Burritos

# Introduction

As the embedded devices are getting popular in daily lives there are prune to more attacks. For secure connected devices its essential to identify the security vulnerabilities in efficient way. Automated fuzz testing is not easy for these devices due to fuzzing speed with limited hardware resources. Existing testing approaches uses re-hosting process by running firmware in an emulated environment which requires human intervention. Earlier reposting approaches didn’t support the monolithic firmware but the modern emulation environments allow re-hosting for monolithic firmware. This process requires a complex task of manually creating software equivalents to run the firmware. Recent strategies are focusing on hardware less re-hosting. There are different strategies like High-level emulation. Pattern-based modeming approaches and deploying guided symbolic execution but all of these strategies allow re-hosting of firmware which has several limitations.

In this paper the authors designed a software-only system Fuzzware which uses combination of light weight program analysis, reposting and fuzz testing together. Fuzzware automatically generates the model by focusing on the inputs that matter, this is done by the identifying useful hardware generated values.

Fuzzware is designed based on the approach that many accesses to the hardware peripherals are short lived such as to check the peripheral’s status and configuration. There is a significant input overhead while querying the unused data. To eliminate the overhead, locally-scoped dynamic symbolic execution(DSE) is used to analyse the meaningful hardware generated data. But as part of the Fuzzware approach they use the generated constraints which geared in input overhead elimination. An emulator is configured using these constraints. The goal of this approach is to present with the original choices with very little overhead. This Fuzzware system has been tested on different hardwares and identified the vulnerabilities in an efficient way. 

# Design

The Fuzzware tool is designed to explore firmware behavior by precisely eliminating both partial and full input overhead. The dynamic symbolic execution technology at the Fuzzware can greatly narrow the range of hardware-generated value. The Fuzzware tool assumes that the target hardware has basic memory mappings such as RAM ranges and the broad MMIO space, and researcher can get the binary firmware image. Also, the attacker is assumed to have complete control over the input to the firmware and uses this to perform the attack.
The design of the Fuzzware tool is to use an additional model on the ISA emulator to simulate MMIO accesses. Therefore, this emulator can skip actual runs and get MMIO access results if there is a suitable model. For those MMIO access that cannot be skipped, Fuzzware will run it in emulator and generate a model for future MMIO accesses.
![overview](https://i.imgur.com/wtGsGwY.png)


# Conclusion

In this paper a novel approach for fuzz testing a monolithic architecture is presented. A software-tool Fuzzware is implemented for automated testing of the secure connected devices to identify security vulnerabilities in an efficient way by eliminating the input overhead and focusing on the firmware logic. Fuzzware tool is evaluated against 77 firmware images for 19 hardware platforms. During the evaluation the tool eliminated upto 95% of input as input overhead and focused only on the relevant 4.5% of hardware generated values. This tool also discovered additional bugs compared to the other approaches. The authors analysed the network stacks of embedded firmware networks ZEPHYR and CONTIKI-NG and found 12 unknown vulnerabilities which were not reported earlier. 
