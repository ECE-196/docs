---
title: Tempature/Gas sensors on ESP32
date: 2025-06-06
authors:
  - name: Leithon Magana
group 7 members:
  - name: Matteo Persiani
  - name: Brigit Quezada
---

![temps](https://github.com/user-attachments/assets/b44e258c-bfcb-4bb0-8968-baa13a49d127)


## Introduction

This tutorial is aiming to accomplish a better grasp of the application of both tempature and gas sensors on an esp32. In this tutorial we will learn about tempature and gas sensors, how they work and how to integrate them using an ESP32 microcontroller. 

### Learning Objectives

- Understand what the sensors read
- PCB design
- Hand soldering sensors on shield
- Connecting shield to esp32


### Background Information

Temperature sensors are devices that measure heat or cold. They convert a physical quantity (temperature) into an electrical signal that can be read by a microcontroller or other electronic device. Carbon Monoxide (CO) sensors are devices designed to detect the presence of carbon monoxide gas in the surrounding atmosphere. CO is a colorless, odorless, and tasteless gas that is highly toxic to humans and animals because it interferes with the blood's ability to carry oxygen. These sensors contain electrodes and an electrolyte. When CO gas comes into contact with the sensor, it causes a chemical reaction that generates a small electrical current. This current is proportional to the concentration of CO in the air. These are two important sensors and with this small amount of current it can be used to send signals into a microcontroller which can be read with one like an esp 32. 

Pros:

- Cost effective
= Integration capabilities
- Low power consumption
- Real time data
- No power consumption when inactive

Cons:

- Accuracy without calibration
- Data interpretation
- Power management
  
Key Concepts:

Calibrating the analog sensors
Building a PCB to hold the sensors and give power to
Soldering said pcb onto a microcontroller like the esp32
Setting up the right pins to receive data

## Getting Started

This tutorial uses the ESP32 S3 mini microcontroller platform with the Arduino IDE. The ESP32 provides built-in WiFi capabilities and multiple GPIO pins, making it ideal for hese sensor projects.

Required downloads and instillations: 
- Arduinbi IDE 2.0: download from arduino.cc
- ESP32 Board Package: add this url, https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json, to the board manager

Required Components: 
- 1 ESP32-S3 Development board
- 1 Tempature Sensor : https://www.digikey.com/en/products/detail/stmicroelectronics/STTS22HTR/10668399?gQT=2

Required Equiptment: 

| Component Name | Quanitity |
| -------------- | --------- |
|ESP32 Dev Board | 1 |
| PCB customized | 1 |
| Tempature sensor | 1 |
| Gas sensor | 1 |
| USB-C (or USB-A) to USB-C Cable | 1 | 


## Part 01: Building the PCB

### Introduction

Start by creating your PCB shield that will be able to connect to the esp32. You would want to create a simple pcb that connect the sensors correctly and are both wire to a 3.3V source (per datasheet) and connect to ground. 

### Objective

- Connect both sensor footprints to 3.3V and ground
- Make sure pins are correct orientation
- Add components (resistors and capacitors) where needed

  ![Tempature Sensor](https://github.com/user-attachments/assets/8ab652f8-beeb-49ae-8f48-de97208e5a9a)


### Background Information

In this tutorial, I learned to create a PCB in KiCad , Digital input/ oputput pin configuration. I understood basic placement of resistsor and capacitor concepts, microcontroller programming fundamentals and digital signal interpretation.

### Components

- ESP32-S3 development board
- KiCad 
- USB-C cable for programming and power
- Computer with Arduino IDE installed

### Soldering parts and Connecting pins

For this tutorial, we start with soldering all the pieces on to the board using soldering paste first. Once all components are placed using the soldering past you want to cook your PCB until fully really. Once that is done you want to make sure that the correct pins are are connecting to the ESP32. 

The reasoning for this check is because certain pins on the esp32 do not take in data like others and if you want to read tempature level the numbers meed to be calibrated and interpreted into degrees.

![SOldered PCB](https://github.com/user-attachments/assets/83d99b1b-919c-4193-80ad-e40a7a286862)

As per the photo making sure that no parts of the soldered components are touching and making sure that the rail on the pcb goes from the 3.3 V to all the other components.


### Testing Sensors

Once you are able to get the shield attached onto the esp32 you must decide which pin you would be testing and see if there are any readings.

Luckiliy for you I have test code to try and test for the Tempature sensor which would be done on arduino. 

            #include < Wire.h >
            #include "STTS22HSensor.h"
            TwoWire        I2CBus = TwoWire(0);
            STTS22HSensor  Temp(&I2CBus);

            void setup() {
            Serial.begin(115200);
            delay(100);
            Serial.println("STTS22H Temperature Test");
            // GPIO37=SDA, GPIO38=SCL
            I2CBus.begin(37, 38, 400000);
            if (Temp.begin() != 0) {
                Serial.println("ERROR: STTS22H not found.");
                while (1) delay(1000);
            }
            Temp.Enable();
            Serial.println("STTS22H initialized!");
            }

            void loop() {
            float temperature;
            if (Temp.GetTemperature(&temperature) == 0) {
                Serial.printf("Temperature: %.2f °C\n", temperature);
            } else {
                Serial.println("Read error");
            }
            delay(1000);
            }

## Analysis
There is a lot that can go wrong when getting ready to use a tempature sensor in so many different places. When creating the PCB shield Truly make sure that all pins are set up corrently in the right orientationa dnthat there is a 3.3 V connection to the sensors. When figuring out which components to use resort to certain datasheets that the sensors should provide. The Process of getting solders on correctly is a tedious one which I think with practice should be really fun and exciting. Once you connect the correct pins to the board and have the shield receive voltage You must use the test code to see what readings you are getting. WIth this tutorial we use STTS22H sensor which will vary reults on different sensors. Following these steps you should get the sensors working just fine. Good luck !!

## Additional Resources and Useful links

(https://youtube.com/shorts/sJJh-J6e2As?si=Xqb7Y2Yls_3wEPgB)

## Similar Tutorial
https://www.instructables.com/Temperature-Sensor-Tutorial/
