---
title: ESP32 WiFi Scanner
date: 2025-19-05
authors:
  - name: Jasmine Le
---
![esp32wifitop](https://github.com/user-attachments/assets/34787a73-4880-49aa-9bcc-420d115f39c9)

## Introduction

This tutorial walks through building a simple **Wi-Fi scanner** using a **ESP32 Dev Board** you have created and a USB-C cable. It will then display them in both the Arduino Serial Monitor and on a simple web server, which this tutorial will walk you through. The project scans for nearby Wi-Fi networks and displays their names and signal strengths through the Serial Monitor. By the end, you’ll know how to set up your board, write and upload code, and understand how Wi-Fi scanning works on the ESP32!

### Learning Objectives

- Understand how the ESP32 performs Wi-Fi network scans
- Set up Arduino IDE and connect your ESP32
- Learn how to use the Serial Monitor to display data as well as on a web server
- Explore `WiFi.h` library functions and network properties to scan for Wi-Fi networks
- Understand 2.4 GHz vs. 5 GHz Wi-Fi limitations
- Make your ESP32 create its own Wi-Fi for demonstration

### Background Information
The ESP32 is a popular microcontroller with built-in Wi-Fi.  
By scanning for nearby networks, you can learn about radio environments, understand Wi-Fi security, and practice displaying real-world data in your code.  
**Note:** The ESP32 can only scan 2.4 GHz Wi-Fi networks, not 5 GHz networks (which is why some campus networks and personal hotspots may not appear if **Maximize Compatibility** is not enabled).


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

### Required Downloads and Installations

[Arduino IDE](https://www.arduino.cc/en/software)

- Arduino is the main environment for writing and uploading code to your ESP32.
- **ESP32 Board Package for Arduino IDE**
  1. Open Arduino IDE → *Preferences*.
  2. In “Additional Board Manager URLs” add:  
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
  3. Go to *Tools > Board > Boards Manager* and search for “ESP32”.  
     Click **Install**.

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
|                |           |
|                |           |

### Required Tools and Equipment

List any tools and equipment you need here.
(Ex, computer, soldering station, etc.)

## Part 01: Name

### Introduction

Briefly introduce what  you are teaching in this section.

### Objective

- List the learning objectives of this section

### Background Information

Give a brief explanation of the technical skills learned/needed
in this challenge. There is no need to go into detail as a
separation document should be prepared to explain more in depth
about the technical skills

### Components

- List the components needed in this challenge

### Instructional

Teach the contents of this section

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
