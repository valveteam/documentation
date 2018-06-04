---
author: Jason Too
lastUpdated: 4-6-2018
email: jyjt3@cam.ac.uk
---


# The Control Module

![The Control Module](https://imgur.com/bj9PgSz.jpg)

## Introduction
The control module was designed to implement part of our team's proposed solution to meet the objectives of the design specification. The module was implemented to interface between the **flow module** and **light module** so that the right flow rates can be achieved alongside with some user debugging features.
This is intended to describe the hardware and software functionality of the **control module** for the *Solar Powered Valve*. 

<!-- TOC -->

- [The Control Module](#the-control-module)
    - [Introduction](#introduction)
    - [Design Process](#design-process)
        - [Design Specifications](#design-specifications)
            - [Functionality](#functionality)
            - [Usability](#usability)
            - [Reliability](#reliability)
            - [Performance](#performance)
            - [Supportability](#supportability)
        - [Design limitations and our solutions](#design-limitations-and-our-solutions)
    - [Implementation](#implementation)
        - [The Hardware](#the-hardware)
                - [Control Module Schematic](#control-module-schematic)
                - [Control Module PCB](#control-module-pcb)
                - [Bill of Materials](#bill-of-materials)
        - [The Software](#the-software)
            - [Tunable functionality](#tunable-functionality)
            - [Adding an integrator to the input](#adding-an-integrator-to-the-input)
            - [The slow PWM code](#the-slow-pwm-code)
            - [Debug features](#debug-features)
    - [Recommendations and further improvements](#recommendations-and-further-improvements)
        - [The better 'slow' PWM](#the-better-slow-pwm)

<!-- /TOC -->

## Design Process
To implement the control module, there were three key design factors to consider and engineer towards. 
* The first is to provide the flow module with the necessary input so that it is able to regulate it's flow. For this we were not certain if the solenoid valve will be able to respond to small changes in voltages sufficiently so we decided to modulate the flow using PWM. Whilst in principle it works, we soon realised that the solenoid valve used had a maximum operating frequency of around 20 Hz, so we implemented our own slow PWM which will be discussed in a [later section](#the-slow-pwm-code).
* Secondly, we had to interface the microcontroller to the light sensor. Using an LDR and connecting it directly to an analog pin simply wouldn't cut it as it would severly limit the length of the cable as well as introduce a substantial amount of noise. Thus we selected the BH1750 for the light module which would communicate via I2C with the Arduino Micro. 
* Lastly, the microcontroller itself should have sufficient features (inputs & outputs) so that the user is able to tweak certain parameters and understand how the microcontroller is behaving given a certain set of inputs. In particular, the control module had to have trimmers so that the upper and lower trigger bounds for flow can be adjusted to cope with different light levels. LEDs were added so that we can monitor the output signal into the flow module and thus, will be able to easily [troubleshoot](#debug-features) any issues we come across. 

### Design Specifications
#### Functionality
* The system sends an input voltage into the **valve module** to control the flow rate using "slow PWM".
* The system is able to measure the the lux levels sent by the **light module** with sufficient precision & reliability over I2C.
* The system is able to provide a linear flow response which is proportionate to the light levels.
* The system has tunable lower and upper thresholds to control the minimum light level to allow flow and maximum light level to completely open the valve.
* The system has tunable pulse widths/duty cycle lengths for easy debugging and can be optimised to improve the reliability of the valve. (the system is no longer restrained by the flow rate as the PWM can be made faster to respond to high flow rates)
* The system boast break out connectors which allows additional functionality to be implemented at a later date. - e.g. flow rate sensor so that a proper PID controller could be put in place to optimise the flow rates.
#### Usability
* The system must have an input voltage from 6V to 20V.
* The system has a logic HIGH value of 5V.
#### Reliability
* The system will always boot up in the correct state to carry out all the functionalities as given above.
* The system should be operational in temperatures between 0-60℃.
#### Performance
* The system is able to respond to changes in light within a second. 
* The system is limited by the performance of the solenoid valve which is only able to cope with frequencies up to 20 Hz. 
* The system software is completely open source and can be altered for other flow regulation uses.
#### Supportability
* The system is built from cheap and readily available parts
### Design limitations and our solutions
During the verification process we stumbled across several limitations in the components and circuits built which would hold back the entire prototype from achieving the proposed objectives namely; 
- The solenoid valve has a maximum frequency of 20Hz so the need for a slow PWM was needed to widen the pulse widths so that lower duty cycles can be achieved reliably. 
- The BH1750 outputs logic HIGH of 3.3V which is 'incompatible' with the Arduino Micro which has a logic HIGH for 5V. (It would still work but long term damage may affect it's reliability) Therefore I2C repeaters was added to the circuits so that the voltages can be step up. This also further improves the maximum operating cable length for the light module which is an added bonus.
- On top of that, a point which was made clear to us at a later stage of development by Dr Kabla, we implemented an integrator to the inputs of the circuit so that it averages across the period of the duty cycle and does not just respond to one particular light level at the end of the period cycle, this gives the system as a whole a more accurate measurement of how much light is incident onto the photocatalytic reactor. 
## Implementation
The control module is split into two main components which are;
- The Hardware which serves are the interface between the software and measuring physical quantities such as light as well as output digital logic values to the flow module
### The Hardware
The control module boasts
* Power regulation circuits to down-convert high unstable voltage from solar panel to 5V and 3.3V required by the rest of the circuit.
* Power Status LEDs for 12V, 5V and 3.3V
* Arduino Micro as the main microcontroller to integrate **light module** & **solenoid module** to meet the problem specification.
* Adjustment and test circuits to help debug and optimise functionality
* Expansion port to allow the addition of another module, with a jumper to select between 5V and 3.3V.
* MOSFET Circuits to isolate 3.3V circuits from 5V circuits.
* I2C Repeaters to isolate capacitance as well as step up I2C voltages from 3.3V to 5V
##### Control Module Schematic
![Control Module Schematic](https://imgur.com/GXw5ZXs.jpg)

The schematic above shows the connections between different components within for the PCB.
##### Control Module PCB
![Control Module Schematic](https://github.com/valveteam/documentation/blob/master/submodules/control_res/control_pcb_box.jpg?raw=true)

The image above shows the PCB layout. Please note the following:
- Ground planes are **not** shown. 
##### Bill of Materials
| Description                                        | Model                | Quantity | Price/Unit |        | 
|----------------------------------------------------|----------------------|----------|------------|--------| 
| 10uF Capacitors                                    | 10uF                 | 4        | £0.15      | £0.59  | 
| Green Power Indicator LEDs                         | LED                  | 4        | £0.24      | £0.96  | 
| I2C Repeater                                       | I2C-PCA9517          | 2        | £1.71      | £3.42  | 
| 6 pin connectors                                   | For Expansion        | 1        | £0.67      | £0.67  | 
| 6 pin connectors                                   | For Flow Module a    | 1        | £0.67      | £0.67  | 
| 6 pin connectors                                   | For Flow Module b    | 1        | £0.67      | £0.67  | 
| 6 pin connectors                                   | For Light Sensor     | 1        | £0.67      | £0.67  | 
| 2 pin connectors                                   | From Solar Panel     | 1        | £0.19      | £0.19  | 
| 2 pin connectors                                   | To Flow Module (F)   | 1        | £0.19      | £0.19  | 
| Inductors                                          | 6.8uH                | 2        | £1.55      | £3.10  | 
| Transistor N-MOSFET (general)                      | NMOS                 | 4        |    £0.19   | £0.74  | 
| Resistor                                           | 4.7k                 | 2        | £0.01      | £0.01  | 
| Resistor                                           | 470                  | 3        | £0.01      | £0.02  | 
| Resistor                                           | 680                  | 1        | £0.01      | £0.01  | 
| Trim-Potentiometer                                 | Trimmer Lower        | 1        | £0.77      | £0.77  | 
| Trim-Potentiometer                                 | Trimmer Upper        | 2        | £0.77      | £1.53  | 
| Arduino Micro                                      | ARDUINO_MICRO_SHIELD | 1        | £16.07     | £16.07 | 
| XP Power 3.3V Switching Regulator (4.75-32V Input) | TR_SERIES            | 1        | £5.41      | £5.41  | 
| XP Power 5V Switching Regulator (4.75-32V Input)   | TR_SERIES            | 1        | £5.41      | £5.41  | 
|                                                    |                      |          |   TOTAL    | £40.36 | 
### The Software
The software uses standard Arduino libraries as well as a BH1750 library for the light sensor. The microcontroller software has four main features. They are as follows;
* [Tunable functionality](#tunable-functionality)
* [Integrated Input](#adding-an-integrator-to-the-input)
* ['Slow' PWM](#the-slow-pwm-code)
* [Serial Debug](#debug-features)
####  Tunable functionality
The three trimmers have output voltages ranging from 0V to 5V. These voltage inputs are then fed into the Arduino's analog input pins to return an integer value between 0 to 1023. We have then integrated this input into the code so that the lower and upper trigger bounds can be varied. 
```cpp
  //Changing pulse width
  // note that analog read is between 0 and 1023
  pWidth = 1000 + (9000.00*analogRead(analogPin3)/1023.00); //goes from 1 Hz to 0.1 Hz.
  
  //tuning circuit
  lBound = analogRead(analogPin1) + 500; // lower bound from 500 - 1523
  uBound = (analogRead(analogPin2)*2) + 19000; //upper bound from 19000 - 21046 
```
#### Adding an integrator to the input
As the light levels may be very variable with animals walking in front of it or just simple overcast, the input was averaged acrossed the period of the duty cycle so that the average light level was then used to determine the duty cycle for the next period.
```cpp
  //BH1750 - reading light levels
  //Averages the light level wrt pWidth
  luxAvg=0;
  while (i<pWidth)
  {
    lux = lightMeter.readLightLevel();
    luxAvg = luxAvg+lux;
    i++;
  };
  luxAvg = luxAvg/pWidth;
  i=0;
```
#### The slow PWM code
The slow PWM code is used to extend the period of the duty cycle so that at lower light levels, the duty cycle is not too small such that it exceeds the 20 Hz limit and thus won't be operational.
```cpp
    //solenoid varying
    tau = pWidth*((lux-lBound)/(uBound-lBound)); //goes to zero when lux is at lBound and to one when lux is at uBound
    digitalWrite(sPin2, HIGH);
    digitalWrite(LED_BUILTIN, HIGH); 
    delay(tau);                       
    digitalWrite(sPin2, LOW);
    digitalWrite(LED_BUILTIN, LOW);
    delay(pWidth-tau);
```
#### Debug features
By using the Arduino IDE, we can obtain the period of the duty cycle, light levels, upper bound, lower bound and the state of the valve using the serial monitor.
```cpp
  Serial.print("Period: ");  
  Serial.print(pWidth/1000);
  Serial.print(" || "); 
  Serial.print("Light: ");  
  Serial.print(luxAvg); 
  Serial.print(" || ");
  Serial.print("Upper Bound: ");  
  Serial.print(uBound);
  Serial.print(" || "); 
  Serial.print("Lower Bound: ");  
  Serial.print(lBound);
  Serial.print(" || ");  
  Serial.print("State of Valve: ");  
  Serial.print(percentage); 
  Serial.println("%"); 
```
***Sample output***
![Serial Output](https://github.com/valveteam/control-module/blob/master/Code/Control/control_bh1750/serial_output.JPG?raw=true)

## Recommendations and further improvements
In our testing the control module satisfies all the design specifications that was defined at the beginning of the document. Indeed this is satisfactory but there are several possible improvements and changes that could be made to improve the design further in terms of costs and simplicity. 
### The better 'slow' PWM
When deployed and after all the debugging and adjustments have been made, a better PWM can be realised by changing the way the current code handles cases at which the pulse width is less than 50ms (20 Hz) by changing the pulse width instead of setting a threshold for the width upon achieving a certain rate. That way, we can technically achieve extremely low duty cycles for really delicate applications.