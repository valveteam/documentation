# The Control Module

## Introduction
This is intended to describe the hardware and software functionality of the **control module** for the *Solar Powered Valve*. 

## Specification

### Functionality
* The system sends an input voltage into the **valve module** to control the flow rate using "slow PWM".
* The system is able to measure the the lux levels sent by the **light module** with sufficient precision & reliability over I2C.
* The system is able to provide a linear flow response which is proportionate to the light levels.
* The system has tunable lower and upper thresholds to control the minimum light level to allow flow and maximum light level to completely open the valve.
* The system has tunable pulse widths/duty cycle lengths for easy debugging and can be optimised to improve the reliability of the valve. (the system is no longer restrained by the flow rate as the PWM can be made faster to respond to high flow rates)
* The system boast break out connectors which allows additional functionality to be implemented at a later date. - e.g. flow rate sensor so that a proper PID controller could be put in place to optimise the flow rates.

### Usability
* The system must have an input voltage from 6V to 20V.
* The system has a logic HIGH value of 5V.

### Reliability
* The system will always boot up in the correct state to carry out all the functionalities as given above.
* The system should be operational in temperatures between 0-60℃.

### Performance
* The system is able to respond to changes in light within a second. 
* The system is limited by the performance of the solenoid valve which is only able to cope with frequencies up to 20 Hz. 
* The system software is completely open source and can be altered for other flow regulation uses.

### Supportability
* The system is built from cheap and readily available parts

## Hardware
The control module boasts
* Power regulation circuits to down-convert high unstable voltage from solar panel to 5V and 3.3V required by the rest of the circuit.
* Power Status LEDs for 12V, 5V and 3.3V
* Ardiuin Micro as the main microcontroller to integrate **light module** & **solenoid module** to meet the problem specification.
* Adjustment and test circuits to help debug and optimise functionality
* Expansion port to allow the addition of another module, with a jumper to select between 5V and 3.3V.
* MOSFET Circuits to isolate 3.3V circuits from 5V circuits.
* I2C Repeaters to isolate capacitance as well as step up I2C voltages from 3.3V to 5V

![Control Module Schematic](https://imgur.com/GXw5ZXs.jpg)

![Control Module PCB](https://imgur.com/bj9PgSz.jpg)

## Software
###  Tunable functionality
```cpp
  //Changing pulse width
  // note that analog read is between 0 and 1023
  pWidth = 1000 + (9000.00*analogRead(analogPin3)/1023.00); //goes from 1 Hz to 0.1 Hz.
  
  //tuning circuit
  lBound = analogRead(analogPin1) + 500; // lower bound from 500 - 1523
  uBound = (analogRead(analogPin2)*2) + 19000; //upper bound from 19000 - 21046 
```
### Adding an integrator to the input
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
### The slow PWM code
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
