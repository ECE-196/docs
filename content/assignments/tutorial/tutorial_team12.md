## Applying a PID controller to ESP32 Tutorial

| Name | Team | Quarter |
| --- | --- | --- |
| Jake Honma | 12 | Spring 2025 |

[Team 12 Website](https://sites.google.com/view/ece-196-sp25/poster)

![Basic PID Feedback Loop](https://tinyurl.com/5857yb2e)

Feedback Loop

## Introduction

- The tutorial will teach students the basics of setting up a PID controller on the ESP32 to control a system to a desired output based on the input and feedback.
- It is important to be able to control a system to get a desired output in projects including temperature, humidity, and motor systems.
- Setting an error term and learning about proportional, integral, and derivative terms in a transfer function.


### Learning Objectives

- Determining error
- How to set Kp, Ki, & Kd
- Basic coding and libraries needed to apply a PID controller on the ESP32

### Background Information

In our project, we are controlling the humidity in different environments (rooms) to counteract potential mold growth. We have an Internet of Things system that senses humidity, sends data to a hub, and activates a dehumidifier. The PID controller is applied to the dehumidifier to converge to the desired humidity level with adequate rise time. These controllers can be applied to multiple systems that require desired outputs based on an input from an actuator. 

- More background info about different terms, transfer functions, and feedback loops

- Include (and cite if needed) any visuals that will help the audience understand.

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| ESP32          |     1     |

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
