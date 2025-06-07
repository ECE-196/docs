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
You should have Arduino IDE installed on your device, Please do so if you haven't (link down below). This is all you need in terms of a software application that is both Mac and Windows supported. You will then use the ESP32 that you have assembled to use for wireless communication on the Wifi Scan tutorial we will be going through.

### Required Downloads and Installations

[Arduino IDE](https://www.arduino.cc/en/software)

- Arduino is the main environment for writing and uploading code to your ESP32.
- **ESP32 Board Package for Arduino IDE**
  1. Open Arduino IDE → *Preferences*.
  2. In “Additional Board Manager URLs” add:  
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
  It should look like this in the correct section:
  <img width="749" alt="Screenshot 2025-06-06 at 9 30 14 PM" src="https://github.com/user-attachments/assets/152b56ce-38b9-466d-999c-05faeec9da85" />

  3. Go to *Tools > Board > Boards Manager* and search for “ESP32”.  
     Click **Install**.

  It should look like this in your Boards Manager (Once installed):
  
  <img width="204" alt="Screenshot 2025-06-06 at 8 47 15 PM" src="https://github.com/user-attachments/assets/0f6d761e-8d25-435e-aa1b-7b39a4e92a93" />

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| ESP32 Dev Board | 1 |
| USB-C Cable | 1 |

### Required Tools and Equipment

List any tools and equipment you need here.
- Computer (Windows, Mac, or Linux)
- Arduino IDE installed

## Part 01: Setting up Arduino & Connecting Your ESP32

### Introduction

In this section, you will install Arduino on your computer (if you haven't already). Once that is done, you will learn to connect your ESP32 to Arduino successfully.

### Objective

- Install and open Arduino IDE
- Add the ESP32 board package
- Select the correct board and port

### Background Information

At this point, you should have Arduino IDE installed on your device. In this section, we will walk through how to connect your ESP32 to Arduino and select the correct board and modules.

### Components

- Computer/Laptop
- ESP32 Board
- USB-C cable

### Instructions

1. **Install Arduino IDE** and open it. If you don't have anything open, you can hover over to File -> New Sketch.
2. **Plug in your ESP32 board** via USB.
4. **Select the correct board:**
   - Since it is your first time, you want to click the dropdown and select "Select other board and port"
       <img width="234" alt="Screenshot 2025-06-06 at 10 53 01 PM" src="https://github.com/user-attachments/assets/3bbd9bac-5e52-41ff-a33e-31b0e4e13c83" />
       
   - Once you get there, you want to select these options on your end, then press "Ok".
     <img width="678" alt="Screenshot 2025-06-06 at 10 55 00 PM" src="https://github.com/user-attachments/assets/c2a647ae-7c26-4d3f-9171-4ba29c071032" />
5. **Make sure the correct port is selected:**  
   - Tools → Port → Select the new device (if not sure, unplug/replug and see which one disappears/reappears).


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
