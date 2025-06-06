---
title: Team 3 ESP32 OLED Web Graphics 
date: 2025 19 May
authors:
  - name: Ferrari Guan 
---

# Team 3: ESP32 OLED Web Graphics 

![esp32](./esp32.jpg)

## Introduction

This tutorial aims to teach you about the wifi capabilities on the ESP32. This is meant to be an extension to the mini project spinning and blinking control system. The motivation behind this is because my ECE 196 final project uses a similar system to control the window cleaning robot traversal. I want readers to learn the lessons from this tutorial and to create a cool project in the process. 

### Learning Objectives

- Using an OLED Screen with the ESP32-S3 Dev board. 
- Using the WIFI capabilities on the ESP32-S3 Dev board. 

### Background Information

This tutorial will show you how to connect an OLED screen to your ESP32 Devboard. This will allow you to create a picture that you can show on the wall. You can remotely change the picture shown on the wall through wifi. Some alternatives would be an IR remote with an IR reader or bluetooth. The IR remote is not very expandable because you need to preset images. However, the IR remote is very easy to implement. Bluetooth is more expandable for future projects, but you need a specific app for compatibility with the ESP32. For this wifi implementation, you have the same expandability of the bluetooth version and the freedom to use your favorite browser at the same time. 

Include (and cite if needed) any visuals that will help the audience understand.

## Getting Started

<!-- For any software prerequisites, write a simple excerpt on each
technology the participant will be expecting to download and install.
Aim to demystify the technologies being used and explain any design
decisions that were taken. Walk through the installation processes
in detail. Be aware of any operating system differences.
For hardware prerequisites, list all the necessary components that
the participant will receive. A table showing component names and
quantities should suffice. Link any reference sheets or guides that
the participant may need.
The following are stylistic examples of possible prerequisites,
customize these for each workshop. -->

### Required Downloads and Installations
<!-- List any required downloads and installations here.
Make sure to include tutorials on how to install them.
You can either make your own tutorials or include a link to them. -->

- Arduino set up from [Lab 2](https://ece-196.github.io/docs/assignments/spinning-and-blinking/)


### Required Components

| Component Name | Quanitity |
| -------------- | --------- |
|ESP32-S3 Dev Board|1|
|USB C Cable|1|
|Adafruit SSD1306 OLED|1|
|Male to Female Jumper Wire|4|

### Required Tools and Equipment

- Computer
- Components from the previous section
- Optional: AC to DC Wall Plug Power Adapter
- Optional: Enclosure Fabrication tool (3D printer, Laser Cutter, CNC Mill, etc)

## Part 01: Set up the OLED display

### Introduction

In this section, you will learn how to connect an OLED display to your ESP32-S3 Dev Board. Next, you will learn now to use Arduino C to interface with the OLED display. 

### Objective

- Learn to wire up an OLED display to an ESP32-S3 
- Learn to code in Arduino C to create images on the OLED display

### Background Information

An Organic Light-Emitting Diode (OLED) display is a type of screen that uses organic compounds to emit light when an electric current is applied. Unlike traditional screens like Liquid Crystal Displays (LCDs), OLED displays do not use a backlight because each pixel emits its own light. So, the OLED displays create stunningly vivid pictures. 

### Components

- ESP32-S3 Dev Board
- 1 Adafruit SSD1306 OLED
- 4 Male to Female Jumper Wires

### Instructional

#### Hardware
Since the OLED display uses I2C to communicate with the ESP32-S3 Dev Board, the OLED display can be directly wired to the ESP32-S3 Dev Board. 

Using your 4 male to female jumper wires, connect the following. 
- Connect the Vin pin on the OLED to the 3.3V pin on the ESP32
- Conenct the GND pin on the OLED to the GND pin on the ESP32
- Connect the SCL pin on the OLED to the 22 pin on the ESP32
- Connect the SDA pin on the OLED to the 21 pin on the ESP32

#### Software

Install the [Adafruit_SSD1306](https://github.com/adafruit/Adafruit_SSD1306) library and [Adafruit_GFX library](https://github.com/adafruit/Adafruit-GFX-Library). 

Open your Arduino IDE and go to Sketch > Include Library > Manage Libraries
<br>
Then, search for all the listed libraries and install them. 

Note: If you are on the legacy version of Arduino IDE, you may need to add the [Adafruit_BusIO](https://github.com/B1Bomber/binaryKeyboard/blob/main/Libraries/Adafruit_BusIO-master.zip) library. 

There are some example code for you to refer to under File > Examples > Adafruit SSD1306
<br>
Connect your ESP32-S3 Dev Board to your computer with a USB C cable and try them out. 

You may read the article in the Sources and Useful Links section (first link) to learn all the functions for the [Adafruit_SSD1306](https://github.com/adafruit/Adafruit_SSD1306) library to create your own pictures. 

### Sources and Useful Links

[OLED display on an ESP32](https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/)

[My Project in a Box Repository](https://github.com/B1Bomber/binaryKeyboard)

## Part 02: Set up the web socket

### Introduction

In this section, you will learn how to set up a web socket on the ESP32-S3 Dev Board. You will be able to connect your web browser to your ESP32-S3 Dev Board. 

### Objective

- Learn to set up a web socket for your ESP32-S3 Dev Board. 

### Background Information

A web socket is a communication protocol that provides independent, simultaneous, and persistent connections between a client (your web browser in this case) and a server (your ESP32 in this case) over a single TCP connection. A web socket is preferred because there is lower latency than HTTP and is persistent, meaning the conneciton will be stable until you explicitly close the connection. 

### Components

- ESP32-S3 Dev Board

### Instructional

Teach the contents of this section

