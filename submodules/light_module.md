---
author: Joshua Efiong
lastUpdated: 05-06-2018
email: je369@cam.ac.uk
---

# Light Module

## Introduction

At a high level, the purpose of the project is to develop a method of controlling water flow in response to varying ambient light intensities. The light sensor module is an independent sub-module of this overall system that is responsible for measuring the ambient light intensity and communicating the measured light intensity to the central control module.

The following documents the design process and chosen implementation for this module, and should enable the reader to undertake further development on this module or to use the device in the field.

## Design

### Technical Specification

A brief technical specification was drawn up to qualify the requirements this sub-module had to meet in order to successfully fulfil its purpose. The following specification is intended to be **solution neutral**, and splits requirements into five groups: functionality, usability, reliability, supportability and performance.

| Requirement Type | Requirement Number | Requirement | Comments |
| :---: | :---: | :---: | :---: | 
| Functionality | LM.FR.01 | The system *must* detect the intensity of ambient light. | The system must have some means to measure the ambient light level, which should give an indication of how effective the photocatalyst in the Majico system is at a given time. |
| Functionality | LM.FR.02 | The system *must* communicate the intensity of ambient light over a digital interface. | The separation of the light sensor module from the central control board was an unknown, but could be in the order of a metre. |
| Functionality | The system *must* be able to cope with flow rates exceeding 6 L/hour. | Given by the Majico photocatalyst flow rate (see specification - add link!)|
| Usability | The system *must* be able to operate in varying sunlight levels. | For example, for an electrical system, this requirement means that the system must accept a range of input voltages (6V-25V D.C. e.g. from a solar panel).|
| Usability | The system *must* be compatible with a range of water supply pressures. | - |
| Reliability | The system *must* be able to operate in temperatures between between 0&deg;C and  60&deg;C. | This gives a marginal around typical temperatures in Tanzania. |
| Reliability | The system *must* be resistant to rain. | There is a monsoon season in Tanzania!|
| Supportability | The system *should* be easily repairable if there is component failure. | - 
| Supportability | The system *should* allow for easy circuit board replacement. | In case of component failure. This requirement mainly relates to an enclosure for such a system. |

### Brainstorming

## Implementation

### Schematic

### PCB Design

### 3D Model

### Light Sensor Devices

#### BH1750

#### VEML6030

### Bill of Materials

## Testing

## Recommendations

### Component Selection

