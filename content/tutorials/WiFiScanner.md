---
title: ESP32 WiFi Scanner
date: 2025-19-05
authors:
  - name: Jasmine Le
---
![esp32wifitop](https://github.com/user-attachments/assets/34787a73-4880-49aa-9bcc-420d115f39c9)

## Introduction

This tutorial walks through building a simple **Wi-Fi scanner** using a **ESP32 Dev Board** you have created and a USB-C cable. It will then display them in both the Arduino Serial Monitor and on a simple web server, which this tutorial will walk you through. The project scans for nearby Wi-Fi networks and displays their names and signal strengths through the Serial Monitor. By the end, you’ll know how to set up your board, write and upload code, and understand how Wi-Fi scanning works on the ESP32!

### Learning Objectives

- Understand how the ESP32 performs Wi-Fi network scans
- Set up Arduino IDE and connect your ESP32
- Learn how to use the Serial Monitor to display data as well as on a web server
- Explore `WiFi.h` library functions and network properties to scan for Wi-Fi networks
- Understand 2.4 GHz vs. 5 GHz Wi-Fi limitations
- Make your ESP32 create its own Wi-Fi for demonstration

### Background Information
The ESP32 is a popular microcontroller with built-in Wi-Fi.  
By scanning for nearby networks, you can learn about radio environments, understand Wi-Fi security, and practice displaying real-world data in your code.  
**Note:** The ESP32 can only scan 2.4 GHz Wi-Fi networks, not 5 GHz networks (which is why some campus networks and personal hotspots may not appear if **Maximize Compatibility** is not enabled).


## Getting Started
You should have Arduino IDE installed on your device, Please do so if you haven't (link down below). This is all you need in terms of a software application that is both Mac and Windows supported. You will then use the ESP32 that you have assembled to use for wireless communication on the Wifi Scan tutorial we will be going through.

### Required Downloads and Installations

[Arduino IDE](https://www.arduino.cc/en/software)

- Arduino is the main environment for writing and uploading code to your ESP32.
- **ESP32 Board Package for Arduino IDE**
  1. Open Arduino IDE → *Preferences*.
  2. In “Additional Board Manager URLs” add:  
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
  It should look like this in the correct section:
  <img width="749" alt="Screenshot 2025-06-06 at 9 30 14 PM" src="https://github.com/user-attachments/assets/152b56ce-38b9-466d-999c-05faeec9da85" />

  3. Go to *Tools > Board > Boards Manager* and search for “ESP32”.  
     Click **Install**.

  It should look like this in your Boards Manager (Once installed):
  
  <img width="204" alt="Screenshot 2025-06-06 at 8 47 15 PM" src="https://github.com/user-attachments/assets/0f6d761e-8d25-435e-aa1b-7b39a4e92a93" />

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| ESP32 Dev Board | 1 |
| USB-C Cable | 1 |

### Required Tools and Equipment

List any tools and equipment you need here.
- Computer (Windows, Mac, or Linux)
- Arduino IDE installed

## Part 01: Setting up Arduino & Connecting Your ESP32

### Introduction

In this section, you will install Arduino on your computer (if you haven't already). Once that is done, you will learn to connect your ESP32 to Arduino successfully.

### Objective

- Install and open Arduino IDE
- Add the ESP32 board package
- Select the correct board and port

### Background Information

At this point, you should have Arduino IDE installed on your device. In this section, we will walk through how to connect your ESP32 to Arduino and select the correct board and modules.

### Components

- Computer/Laptop
- ESP32 Board
- USB-C cable

### Instructions

1. **Install Arduino IDE** and open it. If you don't have anything open, you can hover over to File -> New Sketch.
2. **Plug in your ESP32 board** via USB.
4. **Select the correct board:**
   - Since it is your first time, you want to click the dropdown and select "Select other board and port"
   <img width="234" alt="Screenshot 2025-06-06 at 10 53 01 PM" src="https://github.com/user-attachments/assets/3bbd9bac-5e52-41ff-a33e-31b0e4e13c83" />
       
   - Once you get there, you want to select these options on your end, then press "Ok".
     <img width="678" alt="Screenshot 2025-06-06 at 10 55 00 PM" src="https://github.com/user-attachments/assets/c2a647ae-7c26-4d3f-9171-4ba29c071032" />
5. **Make sure the correct port is selected:**  
   - Tools → Port → Select the new device (if not sure, unplug/replug and see which one disappears/reappears).


## Part 2: Scanning Wi-Fi Networks (Serial Monitor Version)

### Introduction

Let’s write code to make your ESP32 scan and list all nearby Wi-Fi networks in the Serial Monitor.

### Objective

- Understand how to use the `WiFi.h` library
- See the Wi-Fi networks displayed from the Serial Monitor

### Background Information
Now that we have set up Arduino and successfully connected our ESP32 board, we will start to see this Wi-Fi display in action!

### Instructions
1. Assuming you have already opened a new sketch (File -> New Sketch), you should have the following template displayed:
```c
void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}
```
2. You want to first include the Wifi.h library:
```c
#include <WiFi.h>
```
3. Inside your ```void setup()``` function you want to include this block inside:
```c
  Serial.begin(115200);
  delay(1000);

  WiFi.mode(WIFI_STA);  // Station mode is best for scanning
  Serial.println("Scanning for WiFi networks...");
  int n = WiFi.scanNetworks();
  if (n == 0) {
    Serial.println("No networks found.");
  } else {
    Serial.println("Networks found:");
    for (int i = 0; i < n; ++i) {
      Serial.printf("%d: %s (%d dBm) Encryption: %s\n",
        i + 1,
        WiFi.SSID(i).c_str(),
        WiFi.RSSI(i),
        WiFi.encryptionType(i) == WIFI_AUTH_OPEN ? "Open" : "Secured"
      );
    }
  }
```
**What this code does:**
- Sets up Serial Monitor
- Scans for Wi-Fi networks
- Prints each network’s name (SSID), signal strength, and encryption type

5. How to View Results:
  - Click Tools → Serial Monitor 
  - You’ll see the scan results appear in the Serial window
  - Note: Only 2.4 GHz Wi-Fi networks will appear.

You should see something like this for the output in your Serial Monitor:

<img width="619" alt="Screenshot 2025-06-06 at 11 32 49 PM" src="https://github.com/user-attachments/assets/7e80cd29-889a-403a-b422-d020374daa56" />

## Part 3: Scanning and Displaying on a Web Page (ESP32 as Access Point)

### Introduction

Let’s make your ESP32 create its own Wi-Fi network and host a web page that lists all Wi-Fi networks it can see.

### Objective

- Set up the ESP32 in “Access Point” mode
- Build a simple web server to show Wi-Fi scan results in any browser

### Background Information
Now that we have the Wi-Fi networks displaying, let's get a little fancy and host it on a web page!

### Instructions
1. With your existing code from the beginning add this right below the include WiFi.h:
```c
#include <WebServer.h>

const char* ap_ssid = "ESP32-Scanner";
const char* ap_password = "ece196test";

WebServer server(80);
```
This block of code:
- Includes the WebServer library in your program
- The WebServer library lets your ESP32 run a simple website that you can view in your browser
- For ```const char* ap_ssid```, you can change whatever you want in the parentheses, but in this case we are using "ESP32-Scanner"
- The same applies for the password, in this case we are using "ece196test"
```#include <WebServer.h>``` adds the web server functions to your code.

```ap_ssid``` and ```ap_password``` set the Wi-Fi name and password for your ESP32’s own hotspot.

```WebServer server(80);``` sets up a server that will listen for visitors on the standard web port.

2. You will then create a ```void handleRoot()``` function here:
```c
void handleRoot() {
  String page = "<h2>ESP32 Wi-Fi Scanner (AP Mode)</h2>";
  page += "<p>Scanning for networks...</p>";
  int n = WiFi.scanNetworks();
  if (n == 0) {
    page += "<b>No networks found.</b>";
  } else {
    page += "<ol>";
    for (int i = 0; i < n; ++i) {
      page += "<li>";
      page += WiFi.SSID(i);
      page += " (";
      page += WiFi.RSSI(i);
      page += " dBm) - ";
      page += (WiFi.encryptionType(i) == WIFI_AUTH_OPEN ? "Open" : "Secured");
      page += "</li>";
    }
    page += "</ol>";
  }
  page += "<p><button onclick=\"location.reload()\">Refresh</button></p>";
  server.send(200, "text/html", page);
}
```
The ```handleRoot()``` function is responsible for creating and displaying the main web page on your ESP32.
Whenever someone connects to your ESP32’s Wi-Fi and visits its IP address in a browser, this function:
- Scans for all nearby Wi-Fi networks
- Builds an HTML page listing each network’s name, signal strength, and security type
- Adds a Refresh button to allow users to re-scan
- Sends the page to the browser for easy viewing

3. You will want to replace everything in your ```void setup()``` to look like this:
```c
Serial.begin(115200);
  delay(1000);

  WiFi.mode(WIFI_AP);
  WiFi.softAP(ap_ssid, ap_password);
  IPAddress myIP = WiFi.softAPIP();
  Serial.println();
  Serial.print("ESP32 AP IP address: ");
  Serial.println(myIP);
  Serial.print("Wi-Fi name: ");
  Serial.println(ap_ssid);
  Serial.print("Wi-Fi password: ");
  Serial.println(ap_password);

  server.on("/", handleRoot);
  server.begin();
  Serial.println("Web server started. Connect to ESP32 Wi-Fi and visit above IP in your browser.");
```
The web server version of setup() puts the ESP32 in Access Point mode, creates a Wi-Fi network, starts a simple web server, and displays the network and connection info in the Serial Monitor.
The version just scans for nearby Wi-Fi in Station mode and prints results directly to the Serial Monitor with no web interface (See Part 2).

4. In your ```void loop()``` function, you would want to add this line:
```c
server.handleClient();
```
The loop() function repeatedly calls server.handleClient(), which keeps the web server running and responsive. Every time a device tries to connect to your ESP32’s web page, this line ensures the page is served with up-to-date scan results.

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
