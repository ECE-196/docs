---
title: MPU-6050 Sensor communicates with ESP32 Dev Board Tutorial
date: 2025-05-18
authors:
  - name: Sihan Wang
---

![relevant graphic or workshop logo](image/path)

## Introduction

In this tutorial, we will teach how to connect and use the MPU-6050 motion sensor with the ESP32 Dev Board. The sensor gives real-time data about acceleration and rotation, which is useful in many cool projects like balancing robots or motion tracking. 

In addition, we made this tutorial because the MPU-6050 is a low-cost but powerful sensor, we also used this sensor in our own project, which is an anti-theft device for e-scooters. So, we wanted to share what we learned and help other students understand how to get motion data from sensors and use it in their own projects.

By the end of this tutorial, you will know how to:

- Wire the MPU-6050 sensor to the ESP32 Dev Board

- Use **Arduino** to communicate between the sensor and microcontroller

- Get live acceleration and rotation values from the sensor using simple code

### Learning Objectives

- Understand how to connect the MPU-6050 sensor to the ESP32 Dev Board
- Learn how to use the Arduino IDE to upload and run code on the ESP32
- Use provided Arduino code to read motion data (acceleration and gyroscope) from the sensor



### Background Information

The MPU-6050 is a motion-tracking sensor that combines a 3-axis accelerometer and a 3-axis gyroscope into one small chip. The MPU-6050 measures acceleration over the X, Y an Z axis in real time. 


### 📌 MPU-6050 Pinout
![MPU6050 Pinout](sihan_img/MPU6050.png)

Here’s the pinout for the **MPU-6050 sensor module (GY-521 breakout)**. These pins connect to the ESP32 to allow communication and power.

| **Pin** | **Purpose**                | **Connect to ESP32** |
|---------|----------------------------|-----------------------|
| `VCC`     | Power (3.3V or 5V input)   | 3.3V                  |
| `GND`     | Ground                     | GND                   |
| `SCL`     | I²C Clock                  | GPIO 22 (default I²C clock on ESP32)               |
| `SDA`     | I²C Data                   | GPIO 21 (default I²C data on ESP32)               |

**Optional Pins:**

- `INT`: Interrupt output (not used here)  
- `AD0`: Change I²C address (default is 0x68)  
- `XDA`, `XCL`: For extra sensors (ignore for now)


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

You’ll need to install the following tools to program and communicate with the ESP32:
- **Arduino IDE**  
  The Arduino IDE is the platform we’ll use to write and upload code to the ESP32.  
  [Download Arduino IDE](https://www.arduino.cc/en/software)

- **ESP32 Board Package**  
  Add ESP32 support in Arduino IDE. Follow this guide:  
  [ESP32 Setup Instructions](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)

- **MPU-6050 Library**  
  In the Arduino IDE, go to `Tools > Manage Libraries`, then search for and install:  
  `MPU6050` by Electronic Cats (or equivalent).

### Required Components

List your required hardware components and the quantities here.

| **Component Name**       | **Quantity** |
|--------------------------|--------------|
| ESP32 Dev Board          | 1            |
| MPU-6050 (GY-521 Module) | 1            |
| Jumper Wires (Male-Male) | 4            |
| Breadboard *(optional)*  | 1            |

### Required Tools and Equipment

- Computer (Windows, macOS, or Linux)  
- Arduino IDE  
- USB cable (Micro-USB or USB-C depending on your ESP32)  
- Internet connection (for installing tools and libraries)

## Part 01: Setting Up the Circuit – MPU-6050 + ESP32

### Introduction

In this section, we will teach you how to connect the MPU-6050 motion sensor to your ESP32 Dev Board.

### Objective

- Learn how to wire the MPU-6050 sensor to the ESP32 Dev Board
- Understand the use of I²C communication for sensor data transfer
- Prepare the hardware for reading real-time motion data

### Background Information

The MPU-6050 is a 6-axis IMU sensor that combines a 3-axis accelerometer and a 3-axis gyroscope. It uses I²C to send data to the ESP32. I²C only requires two data lines: **SDA (data)** and **SCL (clock)**, making wiring simple and efficient.


### Components

| **Component**          | **Quantity** |
|------------------------|--------------|
| ESP32 Dev Board        | 1            |
| MPU-6050 (GY-521)      | 1            |
| Jumper Wires (Male-Male)| 4           |
| Breadboard             | 1            |

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
