---
title: Controlling Vibration Motor with ESP32
date: 2025-05-19
authors:
  - name: Benjamin Leigh
---

![ ](https://github.com/user-attachments/assets/a63ba139-c374-462b-9d79-05a16f0b7a22)

## Introduction

In this tutorial you will learn how to set up the ESP32 to control a vibration motor. A vibration motor is one of many acutators that you can use to give tactile input to the user.
By the end you should be able to precisely control the intensity and timing of a vibration motor to fulfill your desired purpose.

### Learning Objectives

- Configure the ESP32 Dev board to control a vibration motor
- Use Circuit Python to control the intensity and delay of the vibration motor

### Background Information

A vibration motor is one of the simplest and easiest ways to give applications a way to provide haptic feedback to the user. It is used in many devices that are handheld or worn of the body. One of the most common uses would be the vibration motor in a smartphone to notify the user of an incoming call or update. Vibration motors are advantageous over audio feedback in loud environments, but require the user to be in close contact as a downside.

Pros
- Intuitive, fast feedback
- Reliable and easy to use

Cons
- User has to touch the device to receive feedback
- Limited communication

## Getting Started

For software we will be using Python and Circuit Python in VSCode. Make sure to install the latest versions on your corressponding operating system. 

### Required Downloads and Installations

  [Python in VSCode](https://code.visualstudio.com/docs/languages/python)

  [Circuit Python](https://circuitpython.org/board/espressif_esp32s3_devkitc_1_n8/)
  
### Required Components

| Component Name   | Quanitity |
| ---------------- | --------- |
| ESP32            | 1         |
| Vibration Motor  | 1         |
| Breadboard/PCB   | 1         |
| 220 Ohm Resistor | 1         |
| Wires            | 2         |

Make sure the vibration motor operates in a range from 3V to 5V.

### Required Tools and Equipment

- Computer capable of running VSCode
- USB-C data transferring cable
- Soldering station

## Part 01: Assembling the Vibration Motor

### Introduction

First we will connect a vibration motor directly to the ESP32 Dev board through the use of wires. You can also use a PCB or a breadboard, but this is the simplest way to do it.

### Objective

- Wire vibration motor to ESP32 pins

### Background Information

You will need to know the basics of soldering to complete this step.

### Components

- 1 ESP32 Dev board
- 1 Vibration motor
- 220 Ohm resistor
- Breadboard/PCB
- Wires

### Instructional

First, solder one wire to each of two terminals of the vibration motor. Make sure that the connection is separated and secure. Then, attach one wire to the resistor, and the other side of the resistor to one of the VCC pins on the ESP32 Dev board. Attach the other wire to any one of the IO pins. For the purposes of this tutorial we will be using the IO21 pin, but the other IO pins should work as well.

Here is an example of a PCB setup:

![Image](https://github.com/user-attachments/assets/6d3eb2cd-9f30-4d04-a955-deac4093dbeb)

Here are the pin connections on the ESP32 board:

<img src="https://github.com/user-attachments/assets/b13305bb-e34f-4798-8879-59e4984ae455" alt="Alt text" width="200" height="270"/>

## Part 02: Controlling the Vibration Motor

### Objective

- Control the vibration motor through python in VSCode.

### Background Information

You will need to set up the ESP32 Dev board in the same way that you did with the VU meter.
Tutorial can be found here: [VU Meter](https://ece-196.github.io/docs/assignments/vu-meter/)

### Components

- USB-C data transferring cable
- Computer
- ESP32 Dev board with vibration motor connected
  
### Instructional

After following the steps of the VU meter tutorial up to the initial circuit python code, we will set up the vibration motor in this way:

```python
import board
from digitalio import DigitalInOut, Direction
from time import sleep

# setup pin
vibration_motor = DigitalInOut(board.IO21) # Assign vibration motor to whichever pin you connected it to during assembly
vibration_motor.direction = Direction.OUTPUT

# main loop
while True:
    vib.value = True   # on
    time.sleep(1)
    vib.value = False  # off
    time.sleep(1)

```
This code will toggle the vibration motor between on and off, but what if we wanted to control the intensity? We can do so by importing `pwmio` and change our code like so:

```python
import board
from digitalio import DigitalInOut, Direction
from time import sleep
import pwmio

# setup pin
vibration_motor = pwmio.PWMOut(board.IO21, frequency=1000, duty_cycle=0) #Assign the correct pin you used
int MAX_VAL = 65535 #maximum intensity for pwm

# main loop
while True:
    vibration_motor.duty_cycle = int(0.5 * MAX_VAL) #half intensity
    time.sleep(1)
    vibration_motor.duty_cycle = int(1 * MAX_VAL) #full intensity
    time.sleep(1)

```

As you can see we can now modify the intensity of the vibration motor like so.
And now you are able to precisely configure the vibration motor using the ESP32 Dev board.
Try expirimenting on your own different intensities and time delays for your vibration motor!

### Useful links

[Circuit Python documentation](https://docs.circuitpython.org/en/latest/README.html)

[Vibration Motor datasheet](https://www.vybronics.com/coin-vibration-motors/with-brushes/v-c0720b001f)

[Resistor datasheet](https://www.digikey.com/en/products/detail/stackpole-electronics-inc/RHC2512FT220R/1646043)
