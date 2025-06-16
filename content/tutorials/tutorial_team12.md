---
title: Applying a PID controller to ESP32 Tutorial
date: 2025-05-29
authors:
  - name: Jake Honma
---
[Team 12](https://sites.google.com/view/ece-196-sp25/poster)

![PID Image](https://i.imgur.com/Btq74vr.png)

## Introduction

The tutorial will teach students the basics of setting up a PID controller on the ESP32 to control a system to a desired output based on the input and feedback using Arduino IDE. It is important to be able to control a system to get a desired output in projects, including temperature, humidity, and motor systems. This tutorial should give you a general idea of what PIDs are used for, how to tune them, and how they can be applied to your project!

## Learning Objectives

- Determining error
- Kp, Ki, & Kd and how to tune these variables
- Basic coding and libraries needed to apply a PID controller on the ESP32

## Background Information

We will discuss how to utilize the extension PID_v1 in Arduino IDE to apply a PID controller. Key ideas to know include basic coding skills using Arduino (C++), as well as a basic understanding of feedback loops and transfer functions.

In our project, we are controlling the humidity in different environments (rooms) to counteract potential mold growth. We have an Internet of Things system that senses humidity, sends data to a hub, and activates a dehumidifier. We attempted to apply a PID controller on the dehumidifier to stabilize at a desired RPM, which affects the efficiency of the dehumidifier and humidity levels. These controllers can be applied to multiple systems that require desired outputs based on input from an actuator and a source of feedback. 

## Required Components

### Hardware Components                                     

| Component Name              | Quanitity |                 
| --------------------------- | --------- |
| ESP32                       |     1     |
| Controllable Component      |     1     |
| Feedback Component          |     1     |

### Software Needed 

[Arduino IDE](https://www.arduino.cc/en/software/)

  
# Part 1: PID Overview

## Characteristics of Kp, Kd, & Ki

![Basic PID Feedback Loop](https://tinyurl.com/5857yb2e)

The function, $e$($t$), represents the error, which is found from the difference between the desired output (the difference between the desired output, $r$($t$), and the actual output, $y$($t$). The error signal is fed either forward or backwards (based on the sign of the summation) into the PID controller. 

$K_p$ is the proportional gain, which multiplies the magnitude of the error. $K_i$ acts like a memory term and sums past errors, which is commonly tuned to decrease the accumulation of error. $K_d$ acts to predict future error by considering the rate of change of the error, which is commonly tuned to decrease overshoot 

The output of the error signal going through the PID controller results in a new control signal, $u$($t$), which is fed to the plant (transfer function of the system), such that a new output, $y$($t$), is obtained. This process reiterates that the PID controller is affecting the system.

## Tuning PID Values

![Step Response Info](https://lh3.googleusercontent.com/proxy/4eW_Q3inj-2JT-Wfi37R4nqzs7OBV0NxPbDpTBQUFUqtsldw4Y72p5jqLaOqajDJQ7cenbWrRbiAVBzH2Ag6CdsNqeKg7OvmBVoeUVWdMUaG1-yjq4LlNw)

Rise Time, Overshoot, Settling Time, and Steady-State Error shown above can be adjusted by the following PID coefficient changes:


| Closed-Loop Response   | Rise Time | Overshoot | Settling Time | Steady-State Error |                
| ---------------------- | --------- | --------- |-------------- | ------------------ |
| Kp                     |     decrease     |     increase     |     small changes         |     decrease     |
| Kd                     |     small change     |     decrease     |     decrease         |     no change     |
| Ki                     |     small decrease     |     increase     |     increase         |     decrease (can eliminate)     |

The table indicates how increasing PID coefficients affects system behavior (MAE 143B)

# Part 2: Implementing PIDs in Arduino

## Instructions

Open your Arduino IDE, create a new script, and include the PID library.

    #include <PID_v1.h>

Library for PID commands in Arduino IDE
Pins will need to be defined to send signals (Ex. from a project using a 4-pin CPU Fan & Peltier Device for a functional dehumidifier)

    #define FAN_ENABLE_PIN 42      // Controls fan through a MOSFET
    #define FAN_PWM_PIN    21      // PWM pin for speed control (adjust RPM)
    #define PELTIER_PIN    41      // Controls Peltier through a MOSFET MOSFET
    #define TACH_PIN       26      // Tachometer for RPM feedback

Once pins are set to your desired devices through your ESP32, it is time to initialize variables and PID coefficients.

                                     // in the given example
    double Setpoint = 1310;          // Target RPM
    double Input = 0;                // Measured RPM
    double desiredOutput = 255;      // PWM 0–255

    double Kp = X, Ki = Y, Kd = Z;    // PID coefficients (will need tuning to find right values)

Once all parameters are set, call the PID function within the library
Tip: start with zeroing Ki & Kd and tune for Kp

    PID namePID(double* Input, double* desiredOutput, double* Setpoint, double Kp, double Ki, double Kd, int ControllerDirection)

Example:

    PID fanPID(&currentRPM, &fanPWMOutput, &rpmSetpoint, Kp, Ki, Kd, INDIRECT);
    
Note: ControllerDirection allows for 'DIRECT' & 'INDIRECT', which correlates to feedforward or feedback (sign of the summation for $y$($t$) in picture above)    



    namePID.SetMode(int Mode);                       //AUTOMATIC or MANUAL
    namePID.SetOutputLimits(double Min, double Max); //range of values (for PWM 0-255)

There are functions within the library that are automatically used in the constructor function, PID, but should be called regardless. These functions can be used to update PID values as the system runs to achieve a dynamic response and to set the direction for the internal controller. 

It is recommended to look through the link for further information based on the required use.

[Arduino PID Library Code](https://github.com/br3ttb/Arduino-PID-Library/blob/master/PID_v1.cpp#L97)

### Note:

PID tuning will require experimentation to get the correct PID values for a desired response, which will take time.


## Challenges

- In our system, the CPU Fan had a tachometer (which output a frequency) and had to be converted into RPM and a PWM to send a signal to control the RPM. To find the last and current RPM to apply the PID required additional pulse counting and loops to set up the system.
- There was a lack of fine control on the RPM, meaning that if the PWM needed to increase or decrease, the RPM would change by 100s. This would consistently create a large error due to a natural overshoot, which causes oscillations and never stabilizes.
-   Instead of a PID controller, we opted for hysteresis, which is another control algorithm that has a desired value with set tolerances and only allows the system to be on or off. These tolerances let the system smoothly function and avoid constant on and off switching.
 
## Useful links

[Arduino PID Repository](https://github.com/br3ttb/Arduino-PID-Library)

[Introduction: PID Controller Design - University of Michigan](https://tinyurl.com/ybmbsmmp)

MAE 143B Linear Controls Lecture 8 - Professor Morimoto
