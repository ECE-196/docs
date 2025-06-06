---
title: Magnetic Reed Switches
date: 2025-06-06
authors:
  - name: Brigit Quezada
group 7 members:
  - name: Matteo Persiani
  - name: Leithon Magana
---

![relevant graphic or workshop logo](image/path)

## Introduction

This tutorial is aiming to accomplish a better understanding of the basic functionalities of magnetic reed switches. In this tutorial we will learn about reed switches, how they work and how to integrate them using an ESP32 microcontroller. 

### Learning Objectives

- Understand reed switch operation (NO vs NC)
- Wire reed switches directly to ESP32-S3
- Program basic switch detection
- Monitor switch states via Serial output


### Background Information

A magnetic reed switch is an electronic sensor that acts like a button or a switch. It is activated by the presence by a nearby magnetic field. It consists of two very thin ferromagnetic metal plates (reeds) sealed within a glass envelope filled with inert gas. In a normaly open switch, there is a small gap in between the two plates, so they are not touching eachother. In the presence of a nearby magnetic field, the two plates will bend sligly and come in contact with each other and then close the circuit. IN a normally clsoed switch, the two reed switches naturally have contact with eachother when there is no magnetic field present (this is opposite from the normally open switch). When a magnetic field is applied to these kinds of reed switches, they become magnitied by the same polarity and the magnetic force causes the reeds witches to oull apart, thus openeing the circuit and sopping the current flow. 

Reed switches are used commonly in security systems, door/window sensors and proximity detection. They offer several advantages including no mechanical wear (contactless operation), hermetically sealed contacts that resist corrosion, and long operational life.

Pros:

- No physical contact required for activation
= Very reliable and long-lasting
- Low cost and simple to implement
- Works through non-magnetic materials (plastic, wood, glass)
- No power consumption when inactive

Cons:

- Limited switching speed compared to solid-state sensors
- Can be affected by strong external magnetic fields
  
Key Concepts:

Normally Open (NO): Contacts are open when no magnetic field is present
Normally Closed (NC): Contacts are closed when no magnetic field is present
Actuation Distance: The distance at which the magnet activates the switch
Hysteresis: The difference between activation and deactivation distances

## Getting Started

This tutorial uses the ESP32 S3 mini microcontroller platform with the Arduino IDE. The ESP32 provides built-in WiFi capabilities and multiple GPIO pins, making it ideal for IoT sensor projects.

Required downloads and instillations: 
- Arduinbi IDE 2.0: download from arduino.cc
- ESP32 Board Package: add this url, https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json, to the board manager

Required Components: 
- 1 ESP32-S3 Development board
- 1 Gebildet Pre-Wired Reed Switch (NO/ NC) : https://www.amazon.com/Gebildet-Recessed-Security-Magnetic-Normally/dp/B085XQLQ3N/ref=sr_1_5?dib=eyJ2IjoiMSJ9.T84oAGMrKXDrF4D3rIa_sKrXJ__1yi7gbYOlIRgMjNRn3kpCAzOlAFIaqfbpmV6Xd_wM0ogDDPjGYPP9hbRJAce9fDCl-61y4pl0B8fiK3tAG82CJtm_o5fZ0iZUQQ5cexWFEo5K4YHZ5GQncoKueJfX-fgub_L8VGUf40-aKQYrdASg3PSL0hXhp1rVNQBv3xlcCdIgQpfyVIUs8b4vhr1-BEOgY9W4NR2iT_OH6V0.lz20qwhAakjFqWj13YhidcDCMV3rO8S34D89Al_P4UU&dib_tag=se&keywords=reed%2Bswitch&qid=1745005621&sr=8-5&th=1

Required Equiptment: 
- Computer with USB Port
- USB-C cable for ESP32-S3 Development board

## Part 01: Basic Reed Switch Circuit

### Introduction

Build a simple circuit that detects reed switch activation using only the ESP32-S3 and Serial monitor output.

### Objective

- Connect reed switch directly to ESP32-S3 GPIO pins
- Read and interpret digital switch states in code
- Test switch functionality with magnetic activation

### Background Information

In this tutorial, I learned basic C++ syntax for Arduino IDE, Digital input/ oputput pin configuration, conditional logic. I understood basic pull-up resistsor concepts, microcontroller programming fundamentals and digital signal interpretation.

### Components

- ESP32-S3 development board
- Gebildet pre-wired reed switch (NO/NC configurable)
- USB-C cable for programming and power
- Computer with Arduino IDE installed

### Instructional

For this tutorial, we start with connecting the reed swwitch to the ESP32 S3 development board. To do this, take the blue wire wich is the common termainal and connect it to the GPIO Pin 2 on the ESP32 S3 Development board. Connect the black wire to GND. 

Next, open the Arduino IDE and create a new sketch with this test code: 

const int REED_PIN = 2;

void setup() {
  Serial.begin(115200);
  pinMode(REED_PIN, INPUT_PULLUP);
  Serial.println("Reed Switch Test Starting...");
}

void loop() {
  int switchState = digitalRead(REED_PIN);
  
  if (switchState == HIGH) {  // Magnet present (NC switch opens)
    Serial.println("SWITCH OPEN - Magnet detected!");
  } else {  // No magnet (NC switch closed)
    Serial.println("SWITCH CLOSED - No magnet");
  }
  
  delay(500);  // Check every half second
}

This configures GPIO 2 as an input with the internal pull-up resistor enabled using INPUT_PULLUP mode. This internal resistor makes it so that when the reed switch opens (magnet present), the GPIO pin is pulled HIGH to 3.3V, while when the switch is closed (no magnet), current flows through the switch to ground, making the pin read LOW. The main loop continuously reads the digital state of GPIO 2 using digitalRead(). When the function returns HIGH, it means the magnet has opened the normally closed switch, so we display "Magnet detected!" to the Serial monitor. 

Upload this code to the ESP32-S3, open the Serial Monitor at 115200 baud rate, and test the system by bringing the reed switch pair close togther. The Serial output change from "SWITCH CLOSED - No magnet" to "SWITCH OPEN - Magnet detected!" as you bring them togther and then pull them apart.



### Analysis

My example shows some interesting concepts that are important to understanding reed switches. First it incorperates pin configurations of the blue and black wires. Second it incorperates state change detection. 

## Additional Resources and Useful links

https://www.youtube.com/watch?v=iBjNpcQ4fG8

https://www.samgalope.dev/2025/01/18/using-a-reed-switch-sensor-with-esp32-magnetic-field-detection-made-easy/

https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/761/XBee_Cellular_Garage_Door_Sensor.pdf?_gl=1*1omkzh6*_up*MQ..*_gs*MQ..&gclid=CjwKCAjwo4rCBhAbEiwAxhJlCYtfdXweoyJKg7wQ0OKW3vffvfQxPLxx54j6O2Ih62xsWlhw6O1W7BoCz88QAvD_BwE&gclsrc=aw.ds&gbraid=0AAAAADrbLljsNAWqyFGkERwFblLMZBnEk



