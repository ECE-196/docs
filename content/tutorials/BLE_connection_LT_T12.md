---
title: ESP32 BLE Communication with Notifications
date: 2025-05-28
authors:
  - name: Lukas
  - class: SP25
---

![BLE Diagram](https://tinyurl.com/BLE-img)

## Introduction

This tutorial shows you how to set up Bluetooth Low Energy (BLE) communication using the ESP32-S3 Mini and the Arduino IDE. We'll make the ESP32 advertise itself as a BLE server and send out `"Hello World!"` notifications every second. You'll be able to connect to it with your smartphone using an app like LightBlue or nRF Connect and see live data streaming. This can be used for IOT projects later on, it is especially useful for sending sensor readings, triggering actions, or setting up wireless communication in any embedded project.

## Learning Objectives

- Use Arduino to program BLE functionality on ESP32  
- Create a BLE service and characteristic  
- Send notifications from ESP32 to a mobile device  
- Connect and test BLE output using a smartphone  

## Background Information

BLE stands for Bluetooth Low Energy. Unlike Wi-Fi, it is not meant to transfer large amounts of data. Instead, it's ideal for short-range, low-power communication — perfect for IoT projects. Compared to normal Bluetooth, the energy usage is much smaller.

The ESP32 will act as a BLE **server** and your phone as a **client**. The server advertises a **service** (a category of features), and within that service is a **characteristic** (a piece of data you can read or subscribe to).

Each service and characteristic has a unique **UUID** so the client can understand what it's talking to.

## Overview of our System


[![Untitled-Diagram-drawio.png](https://i.postimg.cc/QxhS664p/Untitled-Diagram-drawio.png)](https://postimg.cc/K4pnj5yz)



---

## Getting Started

Make sure you have the correct software and libraries set up.

### Required Downloads and Installations

- [Arduino IDE](https://www.arduino.cc/en/software)
- ESP32 board manager URL for Arduino Preferences:  
  `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
- BLE scanner app for your smartphone:
  - [LightBlue (iOS)](https://apps.apple.com/us/app/lightblue/id557428110)
  - [nRF Connect (Android)](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp)

### Required Components

| Component              | Quantity |
|------------------------|----------|
| ESP32-S3 Mini          | 1        |
| USB-C cable            | 1        |
| BLE-capable smartphone | 1        |

### Required Tools and Equipment

- Arduino IDE on your computer  
- BLE app installed on your smartphone  

---

## Part 1: Writing the Code

### Introduction

This section walks you through writing the BLE server code that sends out notifications.

### Objective

- Make the ESP32 show up as a BLE device  
- Create a service and characteristic  
- Notify connected clients every second  

### Background Information

We'll use the `BLEDevice`, `BLEServer`, and `BLECharacteristic` classes from the ESP32 BLE library. We’ll define custom UUIDs, which are just unique IDs that help clients know what they're reading.

### Instructions

Paste the following code into the Arduino IDE and upload it to your ESP32-S3 Mini.

````cpp
#include <BLEDevice.h>
#include <BLEServer.h>
#include <BLEUtils.h>
#include <BLE2902.h>

// Declare a pointer for the characteristic
BLECharacteristic *pCharacteristic;

void setup() {
  Serial.begin(115200);  // Start serial monitor for debug messages

  // Initialize BLE and set the device name
  BLEDevice::init("ESP32_S3_Mini");

  // Create a BLE server
  BLEServer *pServer = BLEDevice::createServer();

  // Define a custom service with a 128-bit UUID
  BLEService *pService = pServer->createService("4fafc201-1fb5-459e-8fcc-c5c9c331914b");

  // Create a characteristic within that service
  pCharacteristic = pService->createCharacteristic(
    "beb5483e-36e1-4688-b7f5-ea07361b26a8",
    BLECharacteristic::PROPERTY_READ | BLECharacteristic::PROPERTY_NOTIFY
  );

  // Set initial value and enable notifications
  pCharacteristic->setValue("ESP32 BLE Ready!");
  pCharacteristic->addDescriptor(new BLE2902());

  // Start the BLE service
  pService->start();

  // Begin advertising so clients can find the ESP32
  BLEAdvertising *pAdvertising = BLEDevice::getAdvertising();
  pAdvertising->start();

  Serial.println("BLE advertising started...");
}

void loop() {
  delay(1000);  // Wait for 1 second

  // Create a new message
  String msg = "Hello World!";

  // Update the value of the characteristic
  pCharacteristic->setValue(msg.c_str());

  // Notify connected clients
  pCharacteristic->notify();
}
````
---
 # Part 2: Connecting from Your Phone

### Introductions

Now let’s test the BLE communication using a smartphone app.

### Objective

- Connect to the ESP32 using a BLE app  
- Subscribe to notifications from the characteristic  
- View messages updating every second  

### Background Information

BLE apps like **LightBlue** and **nRF Connect** allow you to view nearby BLE devices, explore their services, and enable notifications to receive live data.

### Instructions

1. Upload the code to your ESP32  
2. Open your BLE scanner app  
3. Scan for devices — look for `ESP32_S3_Mini`  
4. Connect to the device  
5. Find the service UUID: `4fafc201-1fb5-459e-8fcc-c5c9c331914b`  
6. Inside that service, find the characteristic UUID: `beb5483e-36e1-4688-b7f5-ea07361b26a8`  
7. Tap **"Enable Notifications"** or similar  
8. You should start seeing `"Hello World!"` every second  


Here is an example picture of the App interface for our Project. We had the ESP-32 connected to a Sensor which measured Temperature, humidity and particulate matter and send these data via BLE. In your case, the message would be "Hello World!" instead of the sensor output (T:23,H:43.3,P:0.5)

[![IMG-20250523-WA0018.jpg](https://i.postimg.cc/85XLkHd0/IMG-20250523-WA0018.jpg)](https://postimg.cc/nj7X0qP4)


### Sources 

- [Random Nerd Tutorials - ESP32 BLE](https://randomnerdtutorials.com/esp32-bluetooth-low-energy-ble-arduino-ide/)  
- [Electronics Hub - ESP32 BLE Tutorial](https://www.electronicshub.org/esp32-ble-tutorial/)  
- [ArduinoKitProject - BLE with ESP32](https://arduinokitproject.com/bluetooth-low-energy-with-esp32/)
