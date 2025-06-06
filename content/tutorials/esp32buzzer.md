---
title: Buzzing the ESP32
date: 5-19-2025
authors:
  - name: Philip Du
---

![image](https://github.com/user-attachments/assets/cd3e9bc2-550a-4a0e-8cba-34842d4a37eb)


## Introduction

This tutorial is to get you started on your ESP32 and Arduino coding journey. We will start with a speaker to have a more hands on approach to join the bridge between hardware and software. The user should be able to change their code and quickly see that change be outputted based on a different sound that the speaker will produce. This is a great starter project to ease the user into the course for all skill levels. This is a easy way for the user to get accustomed to using the Arduino IDE and components everyone has seen but may have never used. 
### Learning Objectives

- Getting started with Arduino IDE coding
- Learn how to make a sound with a speaker 

### Background Information

We will be introducing coding based on Arduino IDE and how to turn a speaker on as well as change its sound based on frequency. This is a great way to prototype before getting started on designing a PCB. The speaker and led was chosen as these are products everyone has seen or heard of before, but may have never had the chance to work on those parts themselves. 

Pro
- user controlled sound output
- connections to things used everyday

Con
- Could be difficult to scale up
- Components are fairly costly

Key concepts
- Basic Arduino code
- Setting up a circuit
- Output change based on user input 

## Getting Started

### Required Downloads and Installations

Download the required [arduino](https://www.arduino.cc/en/software/) software, we will be doing the coding here

### Required Components

| Component Name | Quanitity |
| -------------- | --------- |
| ESP32          | 1         |
| BreadBoard     | 1         |
| Speaker        | 1         |
| jumper cables  | 6         |
| 220 ohm Resistor  | 1         |
### Required Tools and Equipment

- A laptop with Arduino IDE downloaded
- Depending on the speaker, soldering may be needed
- Usb cable for connections


## Part 01: Setting up the breadboard

### Introduction

We will start this project by placing all of the components needed onto the breadboard and hooking it up to the correct terminals on the ESP32. 

### Objective

- Learn how to set up connections from the breadboard to ESP32
- Learn how to correctly orient a component based on the manufacturer's datasheet 

### Background Information

Soldering a spring contact and a basic understanding of where to place the cables within the ESP32. 

### Components

- ESP32
- Speaker
- LED
- 220 ohm resistor
- jumper cables

### Instructional

We will start by soldering one side of the wires onto the speaker.
![image](https://github.com/user-attachments/assets/231e585b-9bfe-4584-af02-b78938f5624a)

Refer to the datasheet to see which contact is positive. In the case of the speaker I am using, the datasheet states that it is the left contact is the positive one. 
![image](https://github.com/user-attachments/assets/87051209-7678-4ae5-a363-30ba38f2a89d)

Plug in the resistor and the LED onto the breadboard 
![image](https://github.com/user-attachments/assets/14e79375-2fcb-4818-b422-4b0a3c521628)

Next using a cable, plug one side into any of the digital pins on the ESP32 and the other end onto the open side of the resistor
Then on the open side of the LED, we will use the GND pin from the ESP32 and plug that into the GND rail on the breadboard and hook it up into the LED
![image](https://github.com/user-attachments/assets/c0cf3416-a691-4feb-954a-58850bf202ac)

Place the open pins of the speaker into the breadboard, take note of where the positive contact is
Next using a cable, plug one side into any of the digital pins on the ESP32 and the other end into the positive contact of the speaker
Utilizing the GND rail, place the last cable from the rail into the negative contact of the speaker

![image](https://github.com/user-attachments/assets/9b23fb88-4432-46c2-b1b8-ae881d4a57bf)

## Part 02: Setting up the code 

### Introduction

The next part of this project will utilize arduino IDE to get what we placed onto the breadboard to work

### Objective

- Learn how to code in Arduino
- Set up components in Arduino

### Background Information

Now that we have everything hooked up to the ESP32, next we will 

## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
[Speaker Documentation](https://puiaudio.com/file/specs-AS01508MS-SC11-WP-R.pdf)
[USE a BUZZER MODULE (PIEZO SPEAKER) USING ARDUINO UNO](https://projecthub.arduino.cc/SURYATEJA/use-a-buzzer-module-piezo-speaker-using-arduino-uno-cf4191)
