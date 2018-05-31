# Flow Control Module
<!-- TOC -->

- [Flow Control Module](#flow-control-module)
    - [Introduction](#introduction)
    - [The Design Process](#the-design-process)
        - [Technical Specification](#technical-specification)
        - [Brainstorming](#brainstorming)
        - [Key Design Decisions](#key-design-decisions)

<!-- /TOC -->
## Introduction
As you may already know, the purpose of this project is (in short) to develop a method of controlling water flow rate depending on light intensity levels (there is a lot more to it than this - see [the overview](www.google.co.uk)). The flow rate module (as the name might suggest) is an integral sub-module which modules flow rate dependent on some input. 

The purpose of this document is two-fold; not only should it aid you in understanding and using the final implementation that we chose for this sub-module but also details the design process to understand the reasoning behind the decisions that we made. 

## The Design Process
### Technical Specification
The first step undertaken was to draw up a detailed technical specification. The following specification is intended to be **solution neutral**.

**Functional Requirements**
| Requirement| Comments |
| :---: | :---:|
|The system *must* be fail-safe. | In the absence of power, there must be zero flow. This is essential for safety.|
|The system *must* be able to modulate flow rate dependent on some form of input signal. | Signal form unspecified. |
|The system *must* be able to cope with flow rates exceeding 6 L/hour. | Given by the Majico photocatalyst flow rate (see specification - add link!)|

**Usability Requirements**
| Requirement| Comments |
| :---: | :---:|
|The system *must* be able to operate in varying sunlight levels. | For example, for an electrical system, this requirement means that the system must accept a range of input voltages (6V-25V D.C. e.g. from a solar panel).|
|The system *must* be compatible with a range of water supply pressures. | - |

**Reliability Requirements**
| Requirement| Comments |
| :---: | :---:|
|The system *must* be able to operate in temperatures between between 0&deg;C and  60&deg;C. | This gives a marginal around typical temperatures in Tanzania. |
The system *must* be resistant to rain. | There is a monsoon season in Tanzania!|

**Supportability Requirements**
| Requirement| Comments |
| :---: | :---:|
|The system *should* be easily repairable if there is component failure. | - 
|The system *should* allow for easy circuit board replacement. | In case of component failure. This requirement mainly relates to an enclosure for such a system. |

### Brainstorming
The key decision to make is how to module the flow of fluid. Two possible methods of controlling flow rate are as outlined below. 
1. Active Control\
  Fluid can be kept in a reservoir and *pumped* directly to another location. This control is said to be active because with no input, there would be no flow rate.
  
    Advantages:
      - Fail-safe. Without power, fluid is unable to flow (assuming the flow is initially at rest). 
      - Direct control over fluid flow rate - the flow rate is entirely controlled by the pump input and the fluid properties. 

    Disadvantages:
      - High power consumption.

2. Passive Control\
  If fluid is already flowing, its flow can be hindered or slowed down in order to control the flow rate. This technique is passive because the fluid is already moving. 
  
    Advantages:
      - Energy is not expended in moving fluid. It is easy to create fluid flow (e.g. by holding a reservoir at a height) and thus energy expenditure on movement is wasteful. 
      - Easy to create/find (e.g. spring-loaded) systems that are closed when there is no power input and are thus fail-safe. 

    Disadvantages:
      - Need to account for accumulation of fluid at the valve input. 
      - No direct control over instantaneous fluid velocity/flow rate. 

    This method lends itself better to purely mechanical solutions compared to active control. For example, a block of material could expand when heated (suggesting the presence of sunlight) and then block flow rate through a bypass pipe (thus perhaps increasing the flow rate through another pipe). 
      
### Key Design Decisions
At this point, significant thought had been placed into the design and a number of key design decisions were made.         









Author: Mrinank Sharma (`ms2314@cam.ac.uk`)\
Last Updated: 31-5-2018