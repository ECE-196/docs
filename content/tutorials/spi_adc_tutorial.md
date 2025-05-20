
# **ESP32 SPI ADC Tutorial**

### 2025-05-19

### By: Luke Wittemann
___

![image](./Team14Photos/SPI.png "SPI Communication")

## Introduction

In this tutorial, we will be learning to use an analog to digital 
converter, or ADC, which communicates to our ESP32 using the SPI serial interface.

### Learning Objectives



### Background Information



## Getting Started



### Required Downloads and Installations



### Required Components


| Component Name   | Quanitity |
| --------------   | --------- |
|      ESP 32      |      1    |
|Microchip MCP3002 |      1    |

### Required Tools and Equipment

Computer, breadboard, jumper wires

## Part 01: Name

### Introduction



### Objective



### Background Information


### Components

- ESP 32
- Microchip MCP3002 ADC
- Analog input sensor such as a microphone

### Instructional

- Plug the ADC chip, the **MCP3002**, into a breadboard.
- Following the pinout on the ADC's datasheet, connect VDD to the ESP32's 3.3v supply and VSS to ground. Connect
    CLK(SCK), DOUT(MISO), DIN(MOSI), and CS(SS) to pins _, _, _, and _ respectively. These pins can also be altered 
    to any digital pins if the default pins are in use by using SPI.begin(SCK, MISO, MOSI, SS).
- Connect your analog input to CH0 or CH1. This chip can also be used as two inputs or as a single differential
    input with CH0 or CH1 being the negative connection. *That being said, it is usually a good idea to buffer* 
    *this input with a transistor or operational amplifier*. 

## Example

### Introduction


### Example


### Analysis


## Additional Resources

### Useful links
-[ADC part page with datasheet](https://www.microchip.com/en-us/product/MCP3002)
