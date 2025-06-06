---
title: Mini Project 3 - blinking lights to a rhythm
date: 2025-05-19
authors:
  - name: Allan Dong
---



## Introduction

This tutorial will serve as a guide on how to program a esp32 to make a led blink at a certain rhythm. 
I would like readers to learn how to program simple things on the esp32. I wanted to do this since I always liked
programmable led's. 

### Learning Objectives

-python
-esp32 pin control

### Background Information

Any audio file can be seperated into its individual notes easily when converted to .mid(Midi) format. A Esp32 board can be programed to control a variety of actuators and connections. I want to have it blink a led to the rhythm of a song
that the user can upload. This will blink the led to the beat. 

Pros
- Can have a cool visual for some music

Cons
- Songs with more complex beats can be harder to represent with just a few led's

## Getting Started

For any software prerequisites, write a simple excerpt on each
technology the participant will be expecting to download and install.
Aim to demystify the technologies being used and explain any design
decisions that were taken. Walk through the installation processes
in detail. Be aware of any operating system differences.
For hardware prerequisites, list all the necessary components that
the participant will receive. A table showing component names and
quantities should suffice. Link any reference sheets or guides that
the participant may need.
The following are stylistic examples of possible prerequisites,
customize these for each workshop.

With your ESP32 board and usb c connection, you will basically just need a way to connect your led to the pins on the ESP 32 board. 
This can be done either through direct connection or through something like a breadboard. The Esp32 board is actualyl quite complicated and can 
connect to other devices via bluetooth and Wifi, but we are pretty much only working locally for this task. 

### Required Downloads and Installations

circuit python
Arduino IDE(optional)
python mido library

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
|     esp32 board|   1       |
|     led        |   1       |
| lightshield     |   1       |

### Required Tools and Equipment

-computer
-usb-c cable
-solder iron(only if you are directly connecting it)
-breadboard(optional)
-lightshield

## Steps: 
1. Setting up esp32 board
2. Installing software on your computer
3. Programming the esp32 board
4. Getting the software onto your esp32 board

## Part 01: setting up esp32 board
![esp32_board](esp32.jpg)

### Introduction

This part I will show you how to connect a led to the esp32 board, via lightshield in this case. You can also directly connect it to the pins or via a breadboard, but I won't be showing it here since using the lightshield makes it much easier. 

### Objective
 Learn how to connect simple components to the esp32 devboard. 

### Background Information

If you look at the esp32 devboard, you can see that the two sides are lined with pins that can output 3.3V to connected components. While 3.3V can be on the higher side for led's it should still be fine. If it is too bright you can add a small resistor between the devboard and the led to slightly lower the current. 

### Components

1 esp32 devbooard
1 lightshield

### Instructional

Assemble the components together. Connect the lightshield to the esp32 devboard via the header pins on the side.  

![esp32_board_withshield](together.jpg)



## Part 02: Writing the software


### Introduction

This part I will show you how to write the proper code to blink the led's to the rhythm of a uploaded song file

### Objective
 Learn how to convert songs to a usable format for the esp32, then blink led's to the songs beat. 

### Background Information

If you look at the esp32 devboard, you can see that the two sides are lined with pins that can output 3.3V to connected components. While 3.3V can be on the higher side for led's it should still be fine. 

### Components

1 esp32 devboard
1 lightshield
1 computer
1 usb-c cable 

### Instructional
First download your favorite song as a mp3 file. Then convert it to a midi file [here]([https://example.com](https://samplab.com/audio-to-midi)).


## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic



## Part 03: Running the Software


### Introduction

This part I will show you how to properly run the file you just wrote on your esp32

### Objective
 Learn how to run and execute software that you have written on the esp32 deboard. 

### Background Information

Python code on the esp32 runs just like on any other computer. 

### Components

1 esp32 devbooard
1 computer
1 usb-c cable 
1 breadboard connection with led

### Instructional


## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic





