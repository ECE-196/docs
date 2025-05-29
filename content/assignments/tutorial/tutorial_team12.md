# Applying a PID controller to ESP32 Tutorial

May 29, 2025 - Jake Honma - [Team 12](https://sites.google.com/view/ece-196-sp25/poster)

-

Graphic of ESP32 connected to actuator w/ stable response.

-

## Introduction

The tutorial will teach students the basics of setting up a PID controller on the ESP32 to control a system to a desired output based on the input and feedback using Arduino IDE. It is important to be able to control a system to get a desired output in projects, including temperature, humidity, and motor systems. This tutorial should give you a general idea on what PIDs are used for, how to tune them, and how they can be applied to your project!

## Learning Objectives

- Determining error
- Kp, Ki, & Kd and how to tune these variables
- Basic coding and libraries needed to apply a PID controller on the ESP32

## Background Information

We will discuss how to utilize the extension PID_v1 in Arduino IDE to apply a PID controller. Key ideas to know are basic coding skills using Arduino (C++), but it is helpful to have a basic understanding of feedback loops and transfer functions.

In our project, we are controlling the humidity in different environments (rooms) to counteract potential mold growth. We have an Internet of Things system that senses humidity, sends data to a hub, and activates a dehumidifier. The concept was to apply the PID controller to the dehumidifier to stabilize at a desired humidity level with sufficient rise time. These controllers can be applied to multiple systems that require desired outputs based on input from an actuator and a source of feedback. 

## Required Components

### Required Hardware Components

| Component Name              | Quanitity |
| --------------------------- | --------- |
| ESP32                       |     1     |
| Controllable Component      |     1     |
| Feedback Component          |     1     |

### Required Software Needed 

[Arduino IDE](https://www.arduino.cc/en/software/)

  
# Part 1: PID Overview

### Characteristics of Kp, Kd, & Ki

![Basic PID Feedback Loop](https://tinyurl.com/5857yb2e)


### Application / Example



### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
