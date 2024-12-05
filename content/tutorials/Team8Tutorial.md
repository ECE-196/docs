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

Another goal of the project is to integrate both hardware and software to create a  system using the ESP32 microcontroller.
Let's take a dive into the indvidual blocks of code. 

```C
{
  const int batteryPin = 1;
  const float referenceVoltage = 3.3;
  const int resolution = 3950;
  const float voltageDividerRatio = 2.0;
  const float maxBatt = 4.2;
  const float minBatt = 3;
}
```
- `batteryPin` is the analog pin connected to the voltage divider's output
- `referenceVoltage` is the maximum voltage the ADC can read. (normally it's 3.3V for the ESP32)
- `resolution` defines the ADC's bit resolution, which determines how anlog voltage is assigned to digital values
- `voltageDividerRatio` the scaling factor of the voltage divider
- `maxBatt` and `minBatt` defines the battery's voltage range. 

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
    Serial.begin(115200);
    pinMode(?);
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

Your Arduino output should print this several times (your voltage percentage may differ depending on battery).
### Analysis

The example uses an ESP32 and a voltage divider to measure a battery's state of charge. This circuit demostrates the core concepts like ADC functionality and Arduino coding. The voltage divder lowers the battery voltage(3.7V to 1.85V) to a safe level for the ESP32 to read. Once the circuit is connected on the breadboard and linked to the ESP32, the Arduino code will adjust for the voltage scaling to calculate and provide an ouput of both the actual voltage and charge percentage. You can see how this employs the concepts of circuit design and programming with the readBatteryVoltage function by converting ADC readings into functional voltage data. The serial monitor should output real-time voltage and it helps validate the system's accuracy. 

## Challenge Questions

How would this change if we were using a 4.2V 18650 battery? Would you need to change either of the values of the voltage divider or just the code? 

What about a 12V battery?

### Challenge Answers

We would not need to change the voltage divider for a 4.2V battery because 4.2V/2 is 2.1V, which is under the maximum that the board can take in. We would only need to change the minBatt and maxBatt values in the code.

We would need to change the voltage divider for a 12V battery because 12V/2 is 6V, which is nearly double what the board can take in! We could choose R1 = 300kΩ and R2 = 100kΩ to get 1/4 of 12V, or 3V. We would have to change our maxBatt, minBatt, *and VoltageDividerRatio* in the code.

## Additional Resources

### Useful links

