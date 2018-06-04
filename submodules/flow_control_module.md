---
author: Mrinank Sharma
lastUpdated: 4-06-2018
email: ms2314@cam.ac.uk
---
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
        - [The 3D Model](#the-3d-model)
        - [The Valves](#the-valves)
        - [Controlling the Valves](#controlling-the-valves)
            - [L298P](#l298p)
            - [LMD18201](#lmd18201)
        - [Bill of Materials](#bill-of-materials)
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
![Flow Control Module - Schematic](https://raw.githubusercontent.com/valveteam/documentation/master/submodules/flow_control_res/flow-module-schematic.png)

The schematic above shows the connections between different components within for the PCB. Note that:
- There are two different H-Bridge chips; the [`L298P` (click for the datasheet)](https://www.sparkfun.com/datasheets/Robotics/L298_H_Bridge.pdf) is a standard part, commonly used for hobby electronics and is fairly cheap. [`LMD18021T`](http://www.ti.com/lit/ds/symlink/lmd18201.pdf) which is about twice the price of the `L298P` but is more robust in terms of the input voltages it accepts and incorporates some advanced features such as a thermal flag output. Both were placed on the same board to allow each to be tested simultaneously. For each, a terminal block connects the output to the solenoid valve. 
- A terminal block is used to connect the circuit to the power supply. 
- 6-Pin Molex KK connectors are used to interface between the flow control module and the control module.
- There are three indicator LEDS; one to show power from the main power supply (e.g. solar panel), the other two indicate which H-Bridge chip is being used. 

### The PCB
![Flow Control Module - PCB](https://raw.githubusercontent.com/valveteam/documentation/master/submodules/flow_control_res/pcb-annotated.png)

The image above shows the PCB layout. Please note the following:
- Ground planes are **not** shown. 
- The dimensions of the circuit board are (INSERT CORRECT DIMENSIONS HERE).
- The terminal block connecting the PCB to the main power supply is in the top left of the board. 
- The terminal blocks connecting the PCB to each solenoid valve are on the left of the board. The valves do not have a polarity and thus can be connected either way around. 
- **There is an error with this PCB** - the ground pad of the L298P chip should be connected to the ground plane but it is not. 
- In our implementation, each of the status LEDs has been given a different colour.

![Molex Pinout](https://raw.githubusercontent.com/valveteam/documentation/master/submodules/flow_control_res/molex-pinout.png).

The diagram above shows how the Molex KK connectors should be interfaced with other components. Molex KK diagrams reproduced from [MRO Electronics](http://www.mroelectronics.com/mro/product.php?id_product=11670) and [RS](https://uk.rs-online.com/web/p/pcb-headers/4838506).

### The 3D Model
![Flow Control Module - 3D Module](https://raw.githubusercontent.com/valveteam/documentation/master/submodules/flow_control_res/flow-module-3d-model.png)

The diagram above shows a 3D model of the PCB. 

### The Valves
Two off-the-shelf solenoid valves were used with the above PCB:
1. [Adafruit Solenoid Valve](http://cpc.farnell.com/adafruit/997/solenoid-valve-plastic-water-12v/dp/SW04771). This valve has a minimum pressure requirement of 0.03 MP and **liquid** can only flow in one direction. 
2. [RS Hydralectic Solenoid Valve](https://uk.rs-online.com/web/p/solenoid-valves/0342023/). This has a higher minimum pressure requirement but is unidirectional. 

The maximum flow rate for each valve is well above typical photocatalyst flow rates (around 6 L/hour). 

Any solenoid valve can be used with the above PCB. 

### Controlling the Valves
In order to control the valves using the H-Bridges on the PCB, the required inputs must be understood. Correct connections must first be ensured (according to the Molex KK pinout diagram).
#### L298P
The `L298P` requires two digital inputs; input A and input B which are considered to be on if the voltage on the pins exceed 2.3V. There are no additional inputs or outputs. Thus the output of the solenoid valve can be controlled as follows. 

| Input A | Input B| Valve State | 
|  :---:  |  :---: |      :---:     | 
| Low | Low | Closed | 
| High | Low | Open | 
| Low | High | Open | 
| High | High | Closed | 

Note that if both inputs are the same voltage, **the valve is held closed** electrically (similar to a 'brake' mode). This actually consumes power and could be optimised by 'disabling' the H-Bridge when no output is required - more on this later.  

[Link to `L298P` Datasheet](https://www.sparkfun.com/datasheets/Robotics/L298_H_Bridge.pdf).

#### LMD18201
The control using the `LMD18201` component is different. There are three digital inputs which are considered to be high when their voltage exceeds 2V.  There is also an additional output - a thermal flag. 

| Pin| Input / Output? | Comments | 
|  :---:  |  :---: |      :---:     | 
| Brake | Input| If the output is a motor, the motor will brake if the input is high by shorting the terminals. This is not useful behaviour for a solenoid valve and thus **this input should be held low** i.e. connected to GND. | 
| Direction | Input | This pin controls the direction of current flow between output pin 1 and 2. The valves are not polarised and thus the value of this pin is not important. | 
| PWM | High | When this input is high, the solenoid valve is open. Otherwise, it is closed. | 
| Thermal Flag | Output | This output is normally high. However, when the 'junction temperature' exceeds 145&deg;C the output becomes low. This can be used by the control module.| 

[Link to `LMD18021T` Datasheet](http://www.ti.com/lit/ds/symlink/lmd18201.pdf)

### Bill of Materials
The bill of materials for the prototype implemented is as follows. **Note that component links are clickable to see where the component was purchased from**. 

| Component | Value | Quantity | Unit Price (£) | Total Price (£) | 
| :---: |    :---: |  :---: |    :---: |         :---: | 
| [Unpolarised Capacitor](http://uk.farnell.com/murata/grm216r71h103ja01d/cap-mlcc-x7r-10nf-50v-0805/dp/8820090RL)| 10 nF | 2 | 0.08| 0.16 | 
| [Diodes](http://uk.farnell.com/vishay/s07j-gs08/diode-switching-do219/dp/1617736?ost=1617736&scope=partnumberlookahead&exaMfpn=true&searchref=searchlookahead&ddkey=http%3Aen-GB%2FElement14_United_Kingdom%2Fw%2Fsearch) | - | 4 | 0.26 | 1.03 | 
| [LEDs](http://uk.farnell.com/broadcom-limited/hsmg-c170/led-green-1-25mm-x-1-4mm-15mcd/dp/5790852?ost=5790852&ddkey=http%3Aen-GB%2FElement14_United_Kingdom%2Fsearch) | - | 3 | 0.48 | 1.45 |
| [L298P (H-Bridge)](http://uk.farnell.com/stmicroelectronics/l298p/motor-driver-dc-stepper-4a-powerso/dp/2762686?ost=2762686&ddkey=http%3Aen-GB%2FElement14_United_Kingdom%2Fsearch) | - | 1| 4.97 | 4.97 |
| [LMD18201 (H-Bridge)](http://uk.farnell.com/texas-instruments/lmd18201t-nopb/motor-ctrl-half-bridge-3a-to-220/dp/1469249?ost=1469249&ddkey=http%3Aen-GB%2FElement14_United_Kingdom%2Fsearch) | - | -| 12.16| 12.16 |
| [Molex Connectors](https://uk.rs-online.com/web/p/pcb-headers/4838506/) | 01x06 | 2 | 0.27 | 1.54 |
| [Terminal Blocks](https://uk.rs-online.com/web/p/pcb-terminal-blocks/3617667/?relevancy-data=636F3D3126696E3D4931384E525353746F636B4E756D626572266C753D656E266D6D3D6D61746368616C6C26706D3D5E285C647B362C377D5B4161426250705D297C285C647B337D5B5C732D2F255C2E2C5D5C647B332C347D5B4161426250705D3F292426706F3D3126736E3D592673723D2673743D52535F53544F434B5F4E554D4245522677633D4E4F4E45267573743D3336312D37363637267374613D3336313736363726) | 01x02 | 3| 0.45 | 1.35 |
| [Resistor](http://uk.farnell.com/panasonic-electronic-components/erjp06f3000v/res-thick-film-300r-1-0-5w-0805/dp/1894177?ost=1894177&ddkey=http%3Aen-GB%2FElement14_United_Kingdom%2Fsearch)| 300 &#8486; (instead of 330 &#8486;)  | 2 | 0.11| 0.22|
| [Resistor](http://uk.farnell.com/vishay/crcw0805680rfkea/res-thick-film-680r-1-0-125w-0805/dp/1653028?st=680%20ohm%20resistor%200805)| 680 &#8486; | 1 | 0.03| 0.03|
| [(RS) Hydraelectric Solenoid Valve](https://uk.rs-online.com/web/p/solenoid-valves/0342023/?relevancy-data=636F3D3126696E3D4931384E525353746F636B4E756D626572266C753D656E266D6D3D6D61746368616C6C26706D3D5E285C647B362C377D5B4161426250705D297C285C647B337D5B5C732D2F255C2E2C5D5C647B332C347D5B4161426250705D3F292426706F3D3126736E3D592673723D2673743D52535F53544F434B5F4E554D4245522677633D4E4F4E45267573743D3334322D303233267374613D3033343230323326)| - | 1| 12.46| 12.46|
| [Adafruit Solenoid Valve](http://cpc.farnell.com/adafruit/997/solenoid-valve-plastic-water-12v/dp/SW04771?ost=SW04771&ddkey=http%3Aen-CPC%2FCPC_United_Kingdom%2Fsearch)| - |1|7.22| 7.22 |
| **Total** |||| 41.59 | 

## Testing Results
### Valve Testing
Multiple valves were ordered to determine their suitability. An experiment was performed where each valve was connected directly to a power supply and the input voltage was varied. 

## Recommendations
### Improving Our Design
### Choosing Components
