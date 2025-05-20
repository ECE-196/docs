---
title: Getting Started with Accelerometers on ESP32
date: 2025-05-19
authors:
  - name: Linfeng Zhang
---

## Introduction

In this tutorial, we will discuss how to interface the **MPU6050** accelerometer with an **ESP32** development board using the C programming language. I will guide you through several steps to help you successfully read raw data from the accelerometer using the **I2C** protocol.

### Learning Objectives

what will be covered in this tutorial?

- How to write embedded code in **C**
- How to use **ESP32idf** framwork
- How to read and understand **datasheets**
- How to install and use useful VScode extension - **platformIO**
- How to connect MPU6050 with ESP32 dev board through correct **pins**

### Background Information

The ESP32 is a widely used development board, and the MPU6050 accelerometer is a powerful motion detection device that can read 6-axis movement data. With these two devices, we can detect motion for a variety of projects. However, successfully communicating between the MPU6050 and the ESP32 can be a challenging task, as well as it's hard to correctly configure and utilize I2C protocol through C language. A rough outlines of this tutorial will include:

- Step1: Preparation: install useful extension
- Step2: Connect dev board with MPU6050 with wires
- Step3: Read and understand datasheets
- Step4: Example of Code needed to read raw data

![MPU6050 Accelerometer](images-LinfengZhang/mpu6050.jpg)
![ESP32 dev board](images-LinfengZhang/esp32.jpg)

## Getting Started

### Required Downloads and Installations

1. You need to first install VSCode as your coding workspace(add link later)
2. You need to then install an extension called PlatformIO to manage your
project(add link later, also need to add a tutorial how to configure and set up this extension)

### Required Components

| Component Name          | Quanitity |
| wires                   |     4     |
| ESP32 board             |     1     |
| MPU6050 accelerometer   |     1     |
| Bread board             |     1     |
| USB cable               |     1     |

### Required Tools and Equipment

You need a computer and a USB cable to upload your code to your board. You also will need to have the link to MPU6050 datasheet(add link later)

## Step 01: Connect MPU6050 with ESP32 dev board

### Introduction

I will be teaching how to correctly connect pins between ESP32 dev board and MPU6050 accelerometer.

### Objective

- Connect 2 devices using wires and bread board

### Background Information

(will be filled later)

### Components

- A bread board
- 4 wires
- ESP32 dev board
- MPU6050 accelerometer

### Instructional

Correct wirings:
pin 3.3V to pin VCC
pin GND to pin GND
pin IO8 to pin SCL(this may be differ depending on which ESP 32 board you are using)
pin IO9 to pin SDA(same as above)

## Step 02: Read and understand datasheets

## Step 03: Example of Code needed to read raw data

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
