---
Title: IR Car Detector
date: 05/19/2025
authors:
  - name: Fernando Salgado
---


## Introduction

In the mini project, I will be making an obstacle detecting system using an IR obstacle avoidance senor, LED, and Resistor. From this students will be able to learn how to use the Arduino IDE software and write code so that the ESP tells the sensor an obstacle is near. Most people would use a Proximity Sensor to detect an obstacle, this is an alternative!


### Learning Objectives

- Be able to write basic code in Arduino IDE
- Learn ho to use pinouts of ESP32-Mini
- How you can further add to this project

### Background Information

- An IR Obstacle Detector, RED LED, and Resistor will be used together to act as a car parking sensor
- We use the IR Obstacle Detector to use give back a binary value (if a obstacle is present or if its clear)
- In total, there ill be to LED's used, one on the IR Obstacle Detector (green) and one on a seperate breadboard (red). These will be used to to visually represent if a car is present or not.
- An alternative to the IR Obstacle Detector could be an Ultrasonic sensor. Ultrasonic sensors might be preferred if distance meaasurement is needed.
- Before starting, it is important to know how IR detection works. All objects emit heat signatures that cause changes in infared light. These sensors, emit infared light and then detect the reflected radiation to determine if an object is present.

## Getting Started


- Before anything, make sure you have Arduino IDE downloaded. [Use this link to download](https://www.arduino.cc/en/software/)
  -  For Windows click the first options
  -  For MACOS, click the 6th option

The Following are the parts needed for this project:
- IR Obstacle Detector   [For data Sheet Click Here!](https://www.handsontec.com/dataspecs/sensor/IR%20Obstacle%20Detector.pdf)
- ESP32-Mini
- USB Cable
- LED (Perferably Red)
- 220 ohm Resistor
- Jumper Wires (male to male and male to female)
- Breadboard


### Required Downloads and Installations

[For Basic Installation Process of Arduino IDE Click HERE!](https://www.youtube.com/watch?v=ADn67BYMdH0)

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
|       IR obstacle avoidance sensor         |       1    |
|      ESP32-Mini       |     1      |
|        USB cable        |    1       |
|        LED (Perferably Red)        |    1       |
|       220 ohm Resistor         |      1     |
|       Male to Male Jumper Wires         |    4       |
|        Male to Female Jumper Wires        |   4        |

### Required Tools and Equipment

List any tools and equipment you need here.
(Ex, computer, soldering station, etc.)
- Computer (Windows or MAC)
- Very small phillips screwdriver

## Part 01: Name: All in One

### Introduction

In this section you will learn how to wire everything needed!

### Objective

- Learn how to wire IR obstacle detector
- Learn how to wire the ESP32-Mini

### Background Information


- Ensure you now how the polarity of an LED light works. The Cathode, the short end, will be connected to ground, the anode, the longer head, will be connected to power.
- If you are using a different LED, make sure you have the right resistor value to prevent the LED from burning out
- Need to know basic syntax in arduino IDE
## Components


![IR Obstacle Detector](content/tutorials/Breadboard.jpg)
![ESP32 Mini](content/tutorials/Breadboard.jpg)
![Jumper Wires](content/tutorials/Breadboard.jpg)
![LED](content/tutorials/Breadboard.jpg)
![Resistor](content/tutorials/Breadboard.jpg)
![Bread Board](content/tutorials/Breadboard.jpg)


### Instructional

- Step 1: Wire up components
- Step 2: Write code on Arduino IDE
- Step 3: Test

![Image of Everything Wired Up](content/tutorials/Breadboard.jpg)
![Screenshot of Arduino Code Used](content/tutorials/Breadboard.jpg)


### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic
- As you can see, we use the board bread board to be able to provide the 3.3V and GND from the ESP32-Mini to all our other components
- From the code, you will see that the IR Obstacle Detector will give a binary reading, on the serial monitor it shows !Obstacle or Clear depending on if there is an object or not
- The Red Led Used lights up if there is an obstacle (car), meaning a car is detected
- The LED on the sensor, lights up if their is no obstacle (car), meaning no car is detected and space is available

### Useful links

[For More Information on IR Obstacle Detector Pin Out](https://www.handsontec.com/dataspecs/sensor/IR%20Obstacle%20Detector.pdf)
