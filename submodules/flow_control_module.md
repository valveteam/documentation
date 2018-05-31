# Flow Control Module
<!-- TOC -->

- [Flow Control Module](#flow-control-module)
    - [Introduction](#introduction)
    - [The Design Process](#the-design-process)
        - [Technical Specification](#technical-specification)
        - [Brainstorming](#brainstorming)
            - [**How can we control the flow?**](#how-can-we-control-the-flow)
            - [**So why use a valve?**](#so-why-use-a-valve)
            - [**Powering the valve**](#powering-the-valve)
            - [**Interfacing with other modules**](#interfacing-with-other-modules)
        - [Design Summary](#design-summary)
    - [Our Implementation](#our-implementation)
        - [The Schematic](#the-schematic)
        - [The PCB](#the-pcb)
    - [Testing Results](#testing-results)
        - [Valve Testing](#valve-testing)
    - [Recommendations](#recommendations)
        - [Improving Our Design](#improving-our-design)
        - [Choosing Components](#choosing-components)

<!-- /TOC -->
## Introduction
As you may already know, the purpose of this project is (in short) to develop a method of controlling water flow rate depending on light intensity levels (there is a lot more to it than this - see [the overview](www.google.co.uk)). The flow rate module (as the name might suggest) is an integral sub-module which modules flow rate dependent on some input. 

The purpose of this document is two-fold; not only should it aid you in understanding and using the final implementation that we chose for this sub-module but also details the design process to understand the reasoning behind the decisions that we made. 

## The Design Process
### Technical Specification
The first step undertaken was to draw up a detailed technical specification. The following specification is intended to be **solution neutral**.

**Requirements**

| Requirement Type | Requirement| Comments |
| :---: | :---: | :---: | 
| Functionality |The system *must* be fail-safe. | In the absence of power, there must be zero flow. This is essential for safety.|
| Functionality |The system *must* be able to modulate flow rate dependent on some form of input signal. | Signal form unspecified. |
| Functionality |The system *must* be able to cope with flow rates exceeding 6 L/hour. | Given by the Majico photocatalyst flow rate (see specification - add link!)|
| Usability | The system *must* be able to operate in varying sunlight levels. | For example, for an electrical system, this requirement means that the system must accept a range of input voltages (6V-25V D.C. e.g. from a solar panel).|
| Usability | The system *must* be compatible with a range of water supply pressures. | - |
| Reliability | The system *must* be able to operate in temperatures between between 0&deg;C and  60&deg;C. | This gives a marginal around typical temperatures in Tanzania. |
| Reliability | The system *must* be resistant to rain. | There is a monsoon season in Tanzania!|
| Supportability | The system *should* be easily repairable if there is component failure. | - 
| Supportability | The system *should* allow for easy circuit board replacement. | In case of component failure. This requirement mainly relates to an enclosure for such a system. |

### Brainstorming
#### **How can we control the flow?**
The key decision to make is how to module the flow of fluid. Two possible methods of controlling flow rate are as outlined below. 
1. Active Control\
  Fluid can be kept in a reservoir and *pumped* directly to another location.
  
    Advantages:
      - Fail-safe. Without power, fluid is unable to flow (assuming the flow is initially at rest). 
      - Direct control over fluid flow rate - the flow rate is entirely controlled by the pump input and the fluid properties. 

    Disadvantages:
      - High power consumption.

2. Passive Control\
  If fluid is already flowing, its flow can be hindered or slowed down in order to control the flow rate e.g. by moving a plate across a pipe. 
  
    Advantages:
      - Energy is not expended in moving fluid. It is easy to create fluid flow (e.g. by holding a reservoir at a height) and thus energy expenditure on movement is wasteful. 
      - Easy to create/find (e.g. spring-loaded) systems that are closed when there is no power input and are thus fail-safe. 

    Disadvantages:
      - Need to account for accumulation of fluid at the valve input. 
      - Need fluid flow. 
      - No direct control over instantaneous fluid velocity/flow rate. 

    This method lends itself better to purely mechanical solutions compared to active control. For example, a block of material could expand when heated (suggesting the presence of sunlight) and then block flow rate through a bypass pipe (thus perhaps increasing the flow rate through another pipe). However, at this stage of the design, an electrical approach had already been chosen. 
      
#### **So why use a valve?**
The high power draw associated with active control led the team to choose a passive option for flow control. Given that it is very easy to create a fluid flow by holding fluid in an elevated reservoir and that fail-safe solenoid valves are readily available, it was decided that this form of flow control would be appropriate for this project. 

#### **Powering the valve**
Common microcontrollers are very limited in the currents that they are able to supply through their GPIO (general purpose input-output) pins. For example, the Arduino Uno has a maximum D.C. current of 20 mA per I/O (input/output) pin corresponding to a maximum power of 0.1 W (at 5V) ([Technical Specs Here](https://store.arduino.cc/usa/arduino-uno-rev3)). Typical valves have power draws *at least* an order of magnitude greater than this and thus additional circuitry is required to allow the microcontroller to interface with components which require much high currents. 

A simple way of performing this interfacing is to use a [*H-Bridge*](https://en.wikipedia.org/wiki/H_bridge) which is commonly used for bi-directional control of motors. These components are available as a standard package (the `L298P` is common) and these packages provide additional advantages over using a transistor current amplifier such as overtemperature protection. 

#### **Interfacing with other modules**
The valve must be controlled using the control submodule ([*documentation here*](www.google.co.uk/)). H-Bridges require digital inputs in order to control their outputs; that's to say, if the voltage is above a certain threshold (dependent on the specific H-Bridge used), it is considered to be on. Thus GPIO pins from this module can be connected directly to the input pins of any H-Bridge input (especially because distances between these modules are unlikely to be particularly large). Note that some H-Bridges require an logic voltage input (typically around 5V, used to power logic circuits) and this can come directly from the control module whilst the high voltage supply is provided using a seperate, high voltage rail. 

### Design Summary
In summar, the key aspects of the system designed are:
1. The flow will be moduled using a solenoid valve. 
2. A H-Bridge will allow the control module to interface with the flow-control module.  

## Our Implementation
After our design process, we were ready to implement our design. Please note that schematics and PCB layouts were designed using [KiCad](http://kicad-pcb.org/) which is open source! The GitHub repository for the flow control module is found [here](https://github.com/valveteam/flow-module) and contains:

- Schematics (i.e. how each component connects to each other).
- PCB (printed circuit board) layout.
- Additional Gerber and drill files required for PCB manufacture. 

Please note that footprint and component files (or *libraries*, using KiCad terminology) will be required to modify files in the GitHub repository; these can be found in our parts library repository [here](https://github.com/valveteam/parts-library). 

### The Schematic

### The PCB

## Testing Results
### Valve Testing
At the moment, it is unclear 

## Recommendations
### Improving Our Design
### Choosing Components


Author: Mrinank Sharma (`ms2314@cam.ac.uk`)\
Last Updated: 31-5-2018