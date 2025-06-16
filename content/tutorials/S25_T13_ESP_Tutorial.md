---
title: ESP_NOW Two Way Communication Tutorial
date: 2025-05-19
authors:
  - name: Keng-Lien Lin
---

<img width="1266" alt="Screenshot 2025-05-19 at 11 11 28 PM" src="https://github.com/user-attachments/assets/3845e0b0-119c-473a-9e08-e551bdfeee83" />

## Introduction

In this tutorial, you'll learn how to establish two‑way ESP‑NOW communication between multiple ESP32 boards. Rather than relying on a router (Wi‑Fi) or dealing with Bluetooth's limited range and latency, ESP‑NOW offers a lightweight, peer‑to‑peer protocol capable of sending up to 250 bytes per packet. By the end, you'll have a reliable, low‑latency link between your ESP32s for seamless data exchange.

### Learning Objectives

By following this guide, you will:

- Understand the fundamentals of ESP‑NOW and how it compares to Wi‑Fi and Bluetooth
- Discover how to retrieve and use ESP32 MAC addresses for peer setup
- Configure ESP‑NOW peers for one‑to‑one and broadcast messaging
- Implement send and receive callbacks to handle incoming data
- Test and troubleshoot two‑way data transfer on multiple boards

### Background Information

**What is ESP‑NOW?**

ESP‑NOW is a proprietary, connectionless wireless protocol developed by Espressif that allows direct, low‑power communication between ESP32 devices. It bypasses the Wi‑Fi stack, reducing overhead and latency, and doesn't require a network or access point.

**Why use ESP‑NOW?**

- Low latency & power: Ideal for battery‑powered sensors and real‑time controls
- Simple peer setup: Direct MAC‑addressed messaging without DHCP or security keys
- 250‑byte payloads: Sufficient for most sensor readings, commands, and small data packets

**Alternatives**

- Wi‑Fi: High throughput but dependent on routers and networks
- Bluetooth: Good for short‑range but adds pairing complexity and can introduce latency

---

## Getting Started

### Required Downloads and Installations

**Arduino IDE**
Download and install the Arduino IDE from [arduino.cc](https://www.arduino.cc/en/software). This tutorial requires Arduino IDE 1.8.19 or later.

**ESP32 Board Package**
In Arduino IDE, go to File > Preferences and add this URL to Additional Boards Manager URLs:
`https://dl.espressif.com/dl/package_esp32_index.json`

Then go to Tools > Board > Boards Manager, search for "ESP32" and install the package by Espressif Systems.

### Required Components

| Component Name | Quantity |
| -------------- | -------- |
| ESP32 Development Board | 2 or more |
| USB Cable (Type-C or Micro-USB) | 2 |
| Breadboard (optional) | 2 |
| LEDs (optional for visual feedback) | 2 |
| 220Ω Resistors (optional) | 2 |

### Required Tools and Equipment

- Computer with Arduino IDE installed
- USB ports for programming multiple ESP32s

## Part 01: Setting Up Basic ESP-NOW Communication

### Introduction

In this section, we'll establish the foundation for ESP-NOW communication by setting up the basic functions needed for initialization, sending, and receiving data.

### Objective

- Initialize ESP-NOW on ESP32 devices
- Implement callback functions for send and receive operations
- Create a broadcast messaging system

### Background Information

ESP-NOW uses MAC addresses to identify devices. Each ESP32 has a unique MAC address that serves as its identifier in the network. The protocol supports both unicast (one-to-one) and broadcast (one-to-many) communication modes.

### Components

- 2 ESP32 development boards
- USB cables for programming

### Instructional

**Step 1: Create the Communication Library**

First, create the ESP_Communication.cpp file that handles all ESP-NOW operations:

```cpp
#include <Arduino.h>
#include <esp_now.h>
#include <WiFi.h>

// Global variables for message handling
uint8_t messageCounter = 0;
unsigned long lastSendTime = 0;
const unsigned long sendInterval = 3000; // Send every 3 seconds

// Callback function executed when data is sent
void dataSendCallback(const uint8_t *mac_addr, esp_now_send_status_t status) {
    Serial.print("Message Send Status: ");
    Serial.println(status == ESP_NOW_SEND_SUCCESS ? "Success" : "Failed");
}

// Callback function executed when data is received
void dataRecvCallback(const uint8_t *mac_addr, const uint8_t *data, int len) {
    Serial.print("Received from: ");
    for (int i = 0; i < 6; i++) {
        Serial.printf("%02X", mac_addr[i]);
        if (i < 5) Serial.print(":");
    }
    Serial.printf(" | Data: %d\n", *data);
}

void espSetup() {
    WiFi.mode(WIFI_STA);
    Serial.println("Starting ESP-NOW...");
    
    if (esp_now_init() != ESP_OK) {
        Serial.println("Error initializing ESP-NOW");
        return;
    }
    
    esp_now_register_send_cb(dataSendCallback);
    esp_now_register_recv_cb(dataRecvCallback);
    
    Serial.println("ESP-NOW initialized successfully");
    Serial.print("MAC Address: ");
    Serial.println(WiFi.macAddress());
}

void broadcastMessage() {
    unsigned long currentTime = millis();
    
    if (currentTime - lastSendTime >= sendInterval) {
        esp_err_t result = esp_now_send(NULL, &messageCounter, sizeof(messageCounter));
        
        if (result == ESP_OK) {
            Serial.printf("Broadcasting message: %d\n", messageCounter);
        } else {
            Serial.printf("Error sending message: %d\n", result);
        }
        
        messageCounter++;
        lastSendTime = currentTime;
    }
}
```

**Step 2: Create the Main Program**

Create the main ESP_NOW_Tutorial.ino file:

```cpp
#include <Arduino.h>
#include <esp_now.h>
#include <WiFi.h>

// Function prototypes
void espSetup();
void broadcastMessage();
bool addPeer(uint8_t *peerMAC);
void sendMessageToPeer(uint8_t *peerMAC, uint8_t *data, size_t len);

void setup() {
    Serial.begin(115200);
    espSetup();
}

void loop() {
    broadcastMessage();
    delay(100);
}
```

**Step 3: Upload and Test**

1. Connect your first ESP32 and upload the code
2. Open Serial Monitor to see the MAC address
3. Connect your second ESP32 and upload the same code
4. Both devices should start broadcasting and receiving messages

## Part 02: Implementing Targeted Communication

### Introduction

While broadcast messaging is useful, targeted communication allows specific device-to-device messaging, which is more efficient and secure.

### Objective

- Add peer management for targeted messaging
- Implement device-specific communication
- Handle multiple peers simultaneously

### Background Information

ESP-NOW allows up to 20 peers to be registered. Each peer is identified by its MAC address, and you can send messages to specific peers rather than broadcasting to all nearby devices.

### Components

- Same components from Part 01

### Instructional

**Step 1: Add Peer Management**

Modify your ESP_Communication.cpp to include peer management:

```cpp
// Add these functions to your existing code
bool addPeer(uint8_t *peerMAC) {
    esp_now_peer_info_t peerInfo;
    memcpy(peerInfo.peer_addr, peerMAC, 6);
    peerInfo.channel = 0;
    peerInfo.encrypt = false;
    
    if (esp_now_add_peer(&peerInfo) != ESP_OK) {
        Serial.println("Failed to add peer");
        return false;
    }
    Serial.println("Peer added successfully");
    return true;
}

void sendMessageToPeer(uint8_t *peerMAC, uint8_t *data, size_t len) {
    esp_err_t result = esp_now_send(peerMAC, data, len);
    if (result == ESP_OK) {
        Serial.println("Message sent to specific peer");
    } else {
        Serial.println("Error sending message to peer");
    }
}
```

## Example

### Introduction

This example demonstrates a complete two-way communication system where two ESP32 devices exchange sensor data and control commands.

### Example

**Device A (Sender/Controller):**
```cpp
#include <Arduino.h>
#include <esp_now.h>
#include <WiFi.h>

// Replace with your Device B MAC Address
uint8_t deviceB_MAC[] = {0x24, 0x6F, 0x28, 0xAE, 0xC4, 0x40};

typedef struct {
    int sensorValue;
    bool ledCommand;
} message_t;

message_t outgoingMessage;
message_t incomingMessage;

void OnDataSent(const uint8_t *mac_addr, esp_now_send_status_t status) {
    Serial.print("Send Status: ");
    Serial.println(status == ESP_NOW_SEND_SUCCESS ? "Success" : "Fail");
}

void OnDataRecv(const uint8_t *mac, const uint8_t *incomingData, int len) {
    memcpy(&incomingMessage, incomingData, sizeof(incomingMessage));
    Serial.printf("Received - Sensor: %d, LED: %s\n", 
                  incomingMessage.sensorValue, 
                  incomingMessage.ledCommand ? "ON" : "OFF");
}

void setup() {
    Serial.begin(115200);
    WiFi.mode(WIFI_STA);
    
    if (esp_now_init() != ESP_OK) {
        Serial.println("ESP-NOW init failed");
        return;
    }
    
    esp_now_register_send_cb(OnDataSent);
    esp_now_register_recv_cb(OnDataRecv);
    
    // Add peer
    esp_now_peer_info_t peerInfo;
    memcpy(peerInfo.peer_addr, deviceB_MAC, 6);
    peerInfo.channel = 0;
    peerInfo.encrypt = false;
    
    if (esp_now_add_peer(&peerInfo) != ESP_OK) {
        Serial.println("Failed to add peer");
        return;
    }
}

void loop() {
    // Prepare message
    outgoingMessage.sensorValue = analogRead(A0);
    outgoingMessage.ledCommand = (millis() / 1000) % 2; // Toggle every second
    
    // Send message
    esp_err_t result = esp_now_send(deviceB_MAC, (uint8_t *)&outgoingMessage, sizeof(outgoingMessage));
    
    delay(2000);
}
```

### Analysis

This example demonstrates several key concepts:

1. **Structured Data**: Using typedef struct allows sending multiple data types in one message
2. **Bidirectional Communication**: Both devices can send and receive simultaneously
3. **Peer Management**: Specific MAC addresses ensure targeted communication
4. **Callback Functions**: Separate functions handle send confirmation and data reception
5. **Real-time Updates**: The loop continuously sends sensor data and control commands

The system efficiently handles sensor readings, LED control, and status feedback between devices without requiring a central router or complex pairing process.

## Additional Resources

### Useful Links

- [ESP-NOW Two-Way Communication Tutorial](https://randomnerdtutorials.com/esp-now-two-way-communication-esp32/) - Comprehensive guide with additional examples
- [Hiking Board ESP-NOW Extended System](https://github.com/KengLL/Hiking-Board-ESPNOW) - Advanced implementation with message hopping for extended range
- [ESP32 ESP-NOW Official Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html) - Technical reference
- [ESP32 Arduino Core Documentation](https://docs.espressif.com/projects/arduino-esp32/en/latest/) - Arduino-specific ESP32 functions

### Troubleshooting Tips

- Ensure both devices are on the same WiFi channel
- Check MAC addresses are correctly formatted (6 bytes)
- Verify peer addition was successful before sending messages
- Use Serial Monitor to debug callback functions
- Consider power management for battery-powered applications

### Next Steps

- Implement encryption for secure communication
- Add error handling and message acknowledgment
- Create mesh networks for extended range
- Integrate with sensors and actuators for practical applications
