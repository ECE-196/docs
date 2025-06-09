---
title: I2C Tutorial
date: 2025-05-19
author:
  - name: Andrew Dunker
---

# I2C Tutorial

![relevant graphic or workshop logo](.\ADunkerPhotos\I2C-Block-Diagram.jpg)

## Introduction

When designing an electrical system, one of the most common ways to get multiple devices communicating over a wired connection is to use I2C. This is a very common way for sensors to communicate with a main microcontroller/microprocessor. In this tutorial, you should be able to learn how to get devices communicating over I2C fairly easily.

### Learning Objectives

- Students should be able to learn how to wire two devices together to communicate over I2C.
- Students will learn the basics of I2C message structure and addresses.
- Students will be able to have two devices communicating useful information over I2C.

## Required Materials

To follow this tutorial, you will need:

- ESP32-S3 Dev Board
- I2C-compatible sensor (e.g., SHT31, MPU6050)
- Breadboard and jumper wires
- Pull-up resistors (4.7kΩ – 10kΩ)
- USB-C cable for ESP32-S3
- Arduino IDE with ESP32 board support installed

## I2C Basics

I2C (Inter-Integrated Circuit) is a widely used, synchronous serial communication protocol that allows multiple digital devices to communicate over just two shared wires. It is designed for easy communication between ICs on the same board or via short-distance wiring.

The two signal lines used in I2C are:

- **SDA (Serial Data Line)** – used for data exchange between devices.
- **SCL (Serial Clock Line)** – used to synchronize data transfers.

In an I2C system, there is typically one **controller** (the device that initiates communication and manages the clock signal) and one or more **peripherals** (devices that respond to the controller). Each device on the I2C bus is assigned a unique 7-bit address, which the controller uses to select which peripheral it wants to communicate with.

This simple two-wire setup allows multiple sensors, displays, or other modules to be connected using the same data and clock lines, reducing pin usage and wiring complexity.

## Wiring the ESP32-S3 and SEN54

Here's how to wire the Sensirion SEN54 sensor to the ESP32-S3:

| SEN54 Pin | ESP32-S3 Pin |
|-----------|--------------|
| VCC       | 5V           |
| GND       | GND          |
| SDA       | GPIO 8       |
| SCL       | GPIO 9       |

**Note:** Pull-up resistors (4.7-10kΩ) should be placed between 3.3V Pin and both SDA and SCL.

## Example Code for ESP32-S3 with SEN54

```cpp
#include <Wire.h>

#define SDA_PIN 8
#define SCL_PIN 9
#define SEN54_ADDR 0x69 // Default I2C address for SEN54

void setup() {
  Serial.begin(115200);
  Wire.begin(SDA_PIN, SCL_PIN);

  Serial.println("Checking SEN54 connection...");

  Wire.beginTransmission(SEN54_ADDR);
  if (Wire.endTransmission() == 0) {
    Serial.println("SEN54 detected.");
  } else {
    Serial.println("SEN54 not found.");
  }

  delay(1000);
}

void loop() {
  Wire.beginTransmission(SEN54_ADDR);
  Wire.write(0x03); // Example command: Start Measurement (simplified)
  Wire.write(0x00);
  Wire.endTransmission();

  delay(1000);

  Wire.requestFrom(SEN54_ADDR, 6); // Expect 6 bytes for basic test
  while (Wire.available() < 6);

  byte data[6];
  for (int i = 0; i < 6; i++) {
    data[i] = Wire.read();
  }

  Serial.print("Raw data: ");
  for (int i = 0; i < 6; i++) {
    Serial.printf("%02X ", data[i]);
  }
  Serial.println();

  delay(5000);
}
```

This is a simple demonstration to verify communication with the SEN54. For full measurement parsing (e.g., PM2.5, temperature, humidity), refer to Sensirion’s datasheet [here](https://www.mouser.com/datasheet/2/682/Sensirion_Datasheet_Environmental_Node_SEN5x-2934400.pdf)


## Conclusion

Using I2C with the ESP32-S3 Dev Board is a straightforward way to communicate with a wide range of sensors and peripherals. With just two wires and flexible pin configuration, I2C provides a reliable communication method for embedded systems.
