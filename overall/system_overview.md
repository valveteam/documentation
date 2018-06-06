---
author: Mrinank Sharma
lastUpdated: 6-06-2018
email: ms2314@cam.ac.uk
---
# The Overall System
<!-- TOC -->

- [The Overall System](#the-overall-system)
    - [Introduction](#introduction)
    - [Key Requirements](#key-requirements)
    - [Solution Overview](#solution-overview)

<!-- /TOC -->
## Introduction
Hi there!

Just a quick recap: access to drinking water is a major problem in the world. 844 million people don't have access to clean water and 31% of schools don't either (WaterAid). Clearly this isn't okay but [Majico](http://www.majico.org/) have developed a way of solving this - passing water over a photocatalyst. But there is a problem - time that the photocatalyst takes to act on the water depends on the sunlight level and thus some method of modulating flow rate dependant on the light levels is required. To solve this, we have developed a light controller valve. 

The purpose of this document is to provide a brief overview of the system to understand the overall function. 

## Key Requirements
A full specification was drawn for this project; this can be found [here](https://docs.google.com/document/d/1mbmDHw7nYxWOprYOgamT1pU3FCUFVtE7_Fpei4cH_A0/edit?usp=sharing). The key points from this document to note are:

- The system must be fail-safe. In the absence of light *or if anything else goes wrong* there should be no flow.

- The flow control should not be binary. The photocatalyst doesn't either have enough light or not, there is a gradual increase in efficiency (and thus increase in maximum flow rate) as the light levels increase. The solution should thus give a gradual (ideally linear) response terms of the relationship betweeen flow rate and light intensity).

- Adding to the above point, there are also lower and upper thresholds below which no flow is desired and above which the flow should not be increase. However, the exact values for these thresholds may depend on the photocatalyst area, final reaction design, ... There are a number of unknown factors and thus these thresholds must not be set but rather be tunable. 

## Solution Overview
The approach we for this problem is to use solar power, favouring an electrical approach over a mechanical approach; the mix of tunability requirements and an (ideally) linear response ruled out a mechanical approach over the timescales of this project. 

The final prototype we made consists of the following components.

1. The *central control* submodule which reads external sensors and sends out signals. [Find out more...](https://valveteam.github.io/control.html)
2. The *light sensor* submodule which detects light intensity. [Find out more...](https://valveteam.github.io/lighting.html)
3. The *flow control* submodule which modulates flow rate dependent on an electrical input signal. Here, we used a solenoid valve. [Find out more...](https://valveteam.github.io/flow.html)

