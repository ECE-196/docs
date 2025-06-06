---
title: ESP32-S3 Wi-Fi LED Toggle Tutorial
date: 2025-05-19
authors:
  - name: Genaro Salazar Ruiz
---

![ESP32 SoftAP Webserver Diagram](https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2018/07/ESP32-access-point-1.jpg?w=1280&quality=100&strip=all&ssl=1)

## Introduction

This tutorial teaches you how to build a simple web page hosted by a SoftAP that allows you to control two onboard LEDs on the ESP32-S3 DevKit. The board creates its own Wi-Fi network, and you can control the hardware through your phone or browser, no internet required.

### Learning Objectives

- Create a Soft Access Point with the ESP32
- Serve an HTML page from the ESP32 itself
- Toggle GPIO outputs through basic web buttons

### Background Information

The ESP32-S3 is a Wi-Fi-enabled microcontroller that supports hosting its own network using SoftAP mode. Using this, it can serve HTML pages and respond to URL requests. The project focuses on basic GPIO control through the web—a foundational use case for embedded web servers in IoT.

## Getting Started

### Required Downloads and Installations

- [Arduino IDE](https://www.arduino.cc/en/software)
- In Board Manager:
   [esp32 by Espressif](https://github.com/espressif/arduino-esp32)

### Required Components

| Component Name   | Quantity |
|------------------|----------|
| ESP32-S3 DevKit  | 1        |
| USB-C Cable      | 1        |

### Required Tools and Equipment

- Computer with Arduino IDE installed
- Web browser (Chrome, Firefox, etc.)

## Part 01: Wi-Fi LED Control via Web Page

### Objective
- Configure ESP32-S3 as Wi-Fi AP
- Host HTML to control LEDs on GPIO 17 & 18

### Setup
```cpp
#include <WiFi.h>
#include <WebServer.h>

#define LED1_PIN 17
#define LED2_PIN 18

WebServer server(80);

void handleRoot() {
  server.send(200, "text/html", "<html><body>
    <h1>LED Control</h1>
    <a href='/led1/on'>LED 1 ON</a><br>
    <a href='/led1/off'>LED 1 OFF</a><br>
    <a href='/led2/on'>LED 2 ON</a><br>
    <a href='/led2/off'>LED 2 OFF</a>
    </body></html>");
}

void handleLED1On()  { digitalWrite(LED1_PIN, HIGH); server.send(200, "text/plain", "LED1 ON"); }
void handleLED1Off() { digitalWrite(LED1_PIN, LOW);  server.send(200, "text/plain", "LED1 OFF"); }
void handleLED2On()  { digitalWrite(LED2_PIN, HIGH); server.send(200, "text/plain", "LED2 ON"); }
void handleLED2Off() { digitalWrite(LED2_PIN, LOW);  server.send(200, "text/plain", "LED2 OFF"); }

void setup() {
  pinMode(LED1_PIN, OUTPUT);
  pinMode(LED2_PIN, OUTPUT);
  WiFi.softAP("ESP32_LED_AP", "genny123");
  server.on("/", handleRoot);
  server.on("/led1/on", handleLED1On);
  server.on("/led1/off", handleLED1Off);
  server.on("/led2/on", handleLED2On);
  server.on("/led2/off", handleLED2Off);
  server.begin();
}

void loop() {
  server.handleClient();
}
```

## Part 02: Flashing the Code to ESP32-S3

### Tools You’ll Need
- Arduino IDE or VSCode with PlatformIO
- USB-C cable
- ESP32 board support package installed via Board Manager

### Flashing Instructions
1. Open Arduino IDE
2. Go to **Tools > Board > ESP32S3 Dev Module**
3. Connect your ESP32-S3 board via USB
4. Select the correct COM port under **Tools > Port** (usually `esp32` and/or `101`)
5. Paste the Part 01 code into the sketch
6. Press the **Upload** button
7. Open **Serial Monitor** (Ctrl+Shift+M) and check for `HTTP server started` and the IP address!

> ✅ Tip: If upload fails, press and hold the BOOT button while uploading.
>  🛠 Debug Tip: If you see `WiFi.softAP` succeed but can't access the page, check your device's firewall or captive portal settings.

### What You Should See
- A message like `HTTP server started`
- Wi-Fi network name (e.g., `ESP32_LED_AP`) appears in your available networks list
- Serial Monitor should show the AP IP, usually `192.168.4.1`(not all IPs are the same! )

> 🔐 You can customize the access point name and password by changing `ssid` and `password` in your code. Each ESP32 may use a different IP.

## Part 03: Accessing the Web Server

1. Connect to the `ESP32_LED_AP` Wi-Fi using password `genny123`(access keys can be changed to your preference!)
2. Open a browser and visit `http://192.168.4.1`
3. Tap the buttons to toggle LEDs ON/OFF in real time

You can bookmark this page or add it to your phone's home screen for quick access.


## What You Should Have Learned

- How to configure ESP32 as a SoftAP
- How to host a basic HTML control page from the board
- How to flash and debug code using Arduino IDE
- How browser requests map to GPIO state changes
- How to use the Serial Monitor for IP and debug info
- How to build simple, interactive IoT projects with HTTP


## Additional Resources

### Useful links

- [ESP32 Arduino Docs](https://docs.espressif.com/projects/arduino-esp32/en/latest/)
- [ESP32 GitHub Repo](https://github.com/espressif/arduino-esp32)
- [WebServer Library](https://www.arduino.cc/reference/en/libraries/webserver/)
- [ChatGPT reference](https://chatgpt.com/)
