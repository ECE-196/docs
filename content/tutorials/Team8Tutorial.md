---
title: Building a gyroscope using MPU 6000
date: 2024-12-4
authors:
  - name: Jiahao Han
  - name: Ryan Sevidal
---
## Introduction

This tutorial will teach readers how to use the MPU 6000 to measure the x y and z axises to keep upright

### Learning Objectives

- MPU schematics and ways to utilize the chip
- Schematic making, as well as using KiCad to 3d render 
- Arduino C

### Background Information

The MPU chip is primarily used in drones to keep their orientation stable, in your phones, or even in your controllers if you play on it. So, specifically we are trying to do it for drone purposes. We would first need to create the board creating a schematic, then further it by connecting that to a microcontroller that receives the data.

What we're aiming for
1. Making sure that our wiring can power the device on.
2. Provides real-time feedback on where exactly the device is orientated.


Key Concepts:

1. Circuit Basics
2. KiCad Basics
3. Arduino Basics

## Getting Started

You will receive a MPU 6000 Chip that can connect to Arduino IDE

You will also receive a capacitors, wires, and a LTI1962-3.3 voltage regulators. These will be important and maintaing stable voltage throughout the ciruit.

We will be using the Arduino IDE (Integrated Development Environment). This is a free, open source program that allows users to write code and upload it to boards. 

### Required Downloads and Installations

We are going to use KiCad, so if you don't have it installed, click on either KiCad. Now that we have it installed, below is a list of important components.

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| MPU-6000 module   |  1     |
| LT1962-3.3 voltage regulator | 1  |
|Capacitor (1µF)|     1     |
| Capacitor (.01µF)  |     1     |
| Capacitor (10µF)|     1     |
|Capacitor (20µF) |     1     |


### Required Tools and Equipment

Computer, Arduino IDE, KiCad

## Part 01: Setting up the Circuit

### Introduction

In this section, we will be discussing where each part should be wired to.
The data sheet that is attached is the MPU 6000 [LINK](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf). 

### Objective

The data sheet provides some insight on what attaches where. You might have noticed that 
The technical skills learned/needed
in this challenge. There is no need to go into detail as a
separation document should be prepared to explain more in depth
about the technical skills.

### Components

| Component Name | Quanitity |
| -------------- | --------- |
| MPU-6000 module   |  1     |
| LT1962-3.3 voltage regulator | 1  |
|Capacitor (1µF)|     1      |
| Capacitor (.01µF)  |     1     |
| Capacitor (10µF)|     1     |
|Capacitor (20µF) |     1     |

### Instructional

Assemble the circuit as shown in the below image. 
![below image](https://github.com/rsevidal/docs/blob/main/content/tutorials/Team13Photos/Screen%20Shot%202024-12-04%20at%204.59.05%20PM.png)

## Part 02: Writing the Code

### Introduction

In this section, we will be discussing the code used to display the orientation it is facing. Printing out values that allign to 0, and axises that range from its max range to its negative max range.

### Objective

- Understand the Arduino interface.
- Understand how to assign pin numbers in Arduino.
- Understand how to implement functions in Arduino.

### Background Information

This section focuses on coding in the Arduino IDE. The language is very similar to C or C++. If you have any background knowledge in those languages, you will recognize the code and its formatting. If you don't have experience in C/C++, don't worry, we will be breaking down the code into very manageable chunks. 
*Hint: If you don't want to rewrite all the code, use the copy button in the upper right corner of the code blocks!*

### Components

- ESP32 
- USB-C to USB-C Connector cable for ESP32
- MPU printed board from part 1
- Your Computer

### Instructional

Another goal of the project is to integrate both hardware and software to create a system using the ESP32 microcontroller.
Let's first take a look on what to implent in the code

```C
{
  #include <Wire.h>
}
```
- `<Wire.h>` includes functions in the Wire class

```C
{
  float readBatteryVoltage() {
    int rawADC = analogRead(?)
    float voltageAtPin = ?
    float batteryVoltage = ? 
    return batteryVoltage;
  }
}
```

Fill out the code function that calculates the battery voltage.
- `analogRead` captures the raw ADC value corresponding to the voltage at the ESP32 pin.
- `voltageAtPin` using the provided code and understanding of the ADC conversion, write the line of code that calculates it.
  - The ADC reading is a fraction of the max resolution.
  - mulitply this fraction by the reference voltage.
- `batteryVoltage` After finding the voltageAtPin how can we find the actual battery voltage (Hint: remember the scaling factor).

```C
{
  void setup() {
  Wire.begin();
  Serial.begin(?);
  Wire.beginTransmission(?);
  Wire.write(?);
  Wire.write(?);
  Wire.endTransmission();
  }
}
```

- `Serial.begin(115200)` sets up communication with the serial monitor for real-time data output.
- `pinMode` This should configures the ADC pin as an input to read the voltage of the circuit.

```C
{
  void loop() {
    float batteryVoltage = readBatteryVoltage();
    Serial.print("Battery Voltage: ");
    Serial.print(batteryVoltage);
    Serial.println(" V");

    int battPercent = ((batteryVoltage-minBatt)/(maxBatt-minBatt))*100;
    Serial.print("Battery Percentage: ");
    Serial.print(battPercent);
    Serial.println(" %");

    delay(10000);
  }
}
```
- The loop continously monitors and outputs the battery's state:
- `readBatteryVoltage()` gets the current battery voltage
- `Serial.print` displays the voltage in the serial monitor
- `battPercent` calculates the battery's percentage
- `delay(10000)` pauses the loop for 10 seconds

Overall this function should provide us with readings on orentation for all the axises listed.

### Arduino Ouput

![Sample Output](Team13Photos/Arduino_output.png)

Your Arduino output should print this several times (Your output may change with what position it is currently in).
### Analysis

The example uses an ESP32 and a MPU 6000 to measure orientation of x y and z plane, and also collecting the rotational direction x y and z. This circuit demostrates the core concepts like pin compatability and Arduino coding. The board relays the information to the microcontroller, and process that data to a safe level for the ESP32 to read. Once the circuit is connected on and linked to the ESP32, the Arduino code will measure x y and z plane of orientation, and the rotational direction x y and z scalars to calculate and provide an ouput of both the orientation and rotational directions. You can see how this employs the concepts of circuit design and programming with the writing and reading before converting it to data to output back to the Serial monitor.

## Challenge Questions

How would this change if we were using a 4.2V 18650 battery? Would you need to change either of the values of the voltage divider or just the code? 

What about a 12V battery?

### Challenge Answers

We would not need to change the voltage divider for a 4.2V battery because 4.2V/2 is 2.1V, which is under the maximum that the board can take in. We would only need to change the minBatt and maxBatt values in the code.

We would need to change the voltage divider for a 12V battery because 12V/2 is 6V, which is nearly double what the board can take in! We could choose R1 = 300kΩ and R2 = 100kΩ to get 1/4 of 12V, or 3V. We would have to change our maxBatt, minBatt, *and VoltageDividerRatio* in the code.

## Additional Resources

### Useful links

