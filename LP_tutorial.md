Bestest ESP-32 RGB Web Server Tutorial Ever
==============

**Date:** *June 8 2025*

**Author:** *Long Phan*

![RGB!] (RGB.jpg)

## Introduction

This tutorial walks you through turning an ESP32 DevKit into a tiny Wi‑Fi “light mixer.” By the end, you’ll serve a webpage with three sliders that change the Red, Green, and Blue channels of an RGB LED in real‑time. The project shows how easy it is to combine embedded PWM control with HTTP requests so that any phone or laptop becomes a wireless color picker.

### Learning Objectives

- Spin up a minimalist HTTP server on the ESP32 (using the built‑in WiFi.h and WebServer.h libraries or ESPAsyncWebServer).

- Parse slider/GET parameters from a browser and map them to values (0‑255).

- Generate Pulse‑Width‑Modulation (PWM) signals on three GPIO pins.

- Wire and current‑limit a common‑cathode RGB LED.

- Test and debug over Serial and the browser console.

### Background Information

Why RGB LEDs? A single package holds three LEDs, letting you create virtually any color with additive mixing. Controlling color intensity requires fast switching—PWM—something the ESP32’s LEDC hardware handles natively (up to 16 channels, 13‑bit resolution).

Why host a web server? The ESP32 packs Wi‑Fi and an HTTP stack, so your phone becomes both controller and UI. No native app, no cloud—just local control over your LAN. This pattern scales to IoT dashboards, remote sensing, or home‑automation nodes.

Key concepts you’ll meet:

Duty cycle & brightness: LED brightness is proportional to the high‑time of the PWM waveform.

Common‑cathode vs. common‑anode LEDs: We’ll ground the cathode and source current through GPIOs.

mDNS / IP discovery: How clients find the board on your network.

## Getting Started

Below is everything you need to put the project on its feet and running.

### Required Downloads and Installations

|           Tool               | Version |          Purpose          |                         Notes                          |
| ---------------------------- | ------- | ------------------------- | ------------------------------------------------------ | 
| Arduino IDE                  | ≥ 2.0   | Compile & upload firmware | Install the ESP32 by Espressif core via Boards Manager |
| CP210x / CH34x Drivers       | Current | USB‑to‑UART bridge        | Needed if your OS doesn’t detect the DevKit            |
| ESPAsyncWebServer + AsyncTCP | Latest  | Lightweight HTTP server   | Grab them through Library Manager                      |

Quick‑start tip: If you’d rather not install extra libraries, you can stick with the built‑in WebServer.h. Just be aware it is single‑threaded and blocks while sending large pages.

### Required Components

! [Components] (Components.jpeg)

|                Component Name                  | Quanitity |
| ---------------------------------------------- | --------- |
| ESP32 DevKit (any WROOM/WROVER, 30‑ or 38‑pin) |     1     |
| RGB LED – common‑cathode, 4‑pin                |     1     |
| 220 Ω resistors                                |     3     |
| Breadboard & jumper wires                      |   1 set   |
| Micro‑USB cable                                |     1     |

### Required Tools and Equipment

- Computer with USB‑A/C port

- Phone or laptop on the same Wi‑Fi network for testing

## RGB LED Web Server Build

### Introduction

This part walks you through wiring the hardware, flashing the sketch, and verifying that the web interface can change the LED’s colour in real‑time.

### Objective

- Configure Wi‑Fi credentials on the ESP32.

- Serve an HTML page with three colour sliders.

- Convert slider input into PWM duty cycles on three GPIO pins.

- Observe colour changes on the LED instantly.

### Background Information

At its core, the ESP32 acts as both web host and PWM generator. The LEDC peripheral generates high‑speed PWM, while ESPAsyncWebServer handles HTTP requests without blocking the main loop, letting you achieve flicker‑free colour mixing.

### Components

See the Required Components table above – nothing else is needed. 

### Instructional

- Wire the LED – Connect R to GPIO 13, G to GPIO 12, B to GPIO 14, each through a 220 Ω (or 330 Ω) resistor; common cathode to GND.

- Open the sketch – Load the following attached code in Arduino IDE.

- Enter Wi‑Fi credentials – Update ssid and password strings.

- Install libraries – Search “ESPAsyncWebServer” and “AsyncTCP” in Library Manager.

- Upload & open Serial Monitor – Note the IP address when the board connects.

- Navigate to the IP – Move the sliders; colours should shift smoothly.

### Circuit/Wiring

! [Circuit] (Circuit.jpeg)

### Arduino Sketch Code

```ruby
/*
  ESP32 RGB LED Web Server
  Mini-Project #3  –  Long Phan
  ------------------------------------------------------------
*/

#include <WiFi.h>
#include <ESPAsyncWebServer.h>

/* ---------- Wi-Fi credentials ---------- */
const char* ssid     = "YOUR_SSID";
const char* password = "YOUR_PASSWORD";
/* --------------------------------------- */

/* ---------- GPIO / PWM mapping ---------- */
const uint8_t RED_PIN   = 13;   // change if you rewired
const uint8_t GREEN_PIN = 12;
const uint8_t BLUE_PIN  = 14;

const uint8_t  RED_CH   = 0;
const uint8_t  GREEN_CH = 1;
const uint8_t  BLUE_CH  = 2;

const uint16_t PWM_FREQ = 5000;   // 5 kHz carrier
const uint8_t  PWM_BITS = 8;      // 8-bit duty-cycle resolution (0-255)
/* ---------------------------------------- */

AsyncWebServer server(80);

/* ---------- Single-page HTML UI ---------- */
const char PAGE[] PROGMEM = R"HTML(
<!DOCTYPE html>
<html>
<head><meta charset="utf-8">
<title>ESP32 RGB LED</title>
<style>
 body{font-family:sans-serif;text-align:center;margin-top:40px}
 input{width:80%%;max-width:380px;margin:8px 0}
</style>
</head>
<body>
 <h2>ESP32 RGB Controller</h2>
 R <input type="range" id="r" min="0" max="255"><br>
 G <input type="range" id="g" min="0" max="255"><br>
 B <input type="range" id="b" min="0" max="255"><br>

<script>
function send(){
 fetch(`/set?r=${r.value}&g=${g.value}&b=${b.value}`);
}
['r','g','b'].forEach(id=>document.getElementById(id).oninput=send);
</script>
</body>
</html>
)HTML";
/* ---------------------------------------- */

/* ===== PWM helper ===== */
void setupPWM() {
  ledcSetup(RED_CH,   PWM_FREQ, PWM_BITS);
  ledcAttachPin(RED_PIN,   RED_CH);

  ledcSetup(GREEN_CH, PWM_FREQ, PWM_BITS);
  ledcAttachPin(GREEN_PIN, GREEN_CH);

  ledcSetup(BLUE_CH,  PWM_FREQ, PWM_BITS);
  ledcAttachPin(BLUE_PIN,  BLUE_CH);

  // start with LED off
  ledcWrite(RED_CH,   0);
  ledcWrite(GREEN_CH, 0);
  ledcWrite(BLUE_CH,  0);
}

/* ===== HTTP /set handler ===== */
void handleSet(AsyncWebServerRequest* req) {
  if (req->hasParam("r")) ledcWrite(RED_CH,   req->getParam("r")->value().toInt());
  if (req->hasParam("g")) ledcWrite(GREEN_CH, req->getParam("g")->value().toInt());
  if (req->hasParam("b")) ledcWrite(BLUE_CH,  req->getParam("b")->value().toInt());
  req->send(204);                       // 204 No Content
}

/* ===== Wi-Fi connect ===== */
void connectWiFi() {
  WiFi.begin(ssid, password);
  Serial.print("Connecting to Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(400);
    Serial.print('.');
  }
  Serial.print("\nConnected! IP = ");
  Serial.println(WiFi.localIP());
}

/* ===== Arduino hooks ===== */
void setup() {
  Serial.begin(115200);

  setupPWM();
  connectWiFi();

  server.on("/",   HTTP_GET,
            [](AsyncWebServerRequest* request){
                request->send_P(200, "text/html", PAGE);
            });
  server.on("/set", HTTP_GET, handleSet);

  server.begin();
}

void loop() {
  // nothing here – AsyncWebServer does its own polling
}
```
### Final Product!

! [Done] (done.jpeg)

## Example

### Introduction

Let’s mix a teal colour (R = 0, G = 128, B = 128) to prove everything works.

### Example

- Open the web page and drag Green to ~50 %.

- Drag Blue to ~50 %.

- Leave Red at 0 %.

- The LED should glow a pleasant teal.

! [teal] (teal.jpeg)

### Analysis

The sliders sent a GET request like /slider?r=0&g=128&b=128. In the handler, those values were written directly to PWM channels 1 and 2, giving equal mid‑level duty cycles on the green and blue dies and none on red, producing teal.

## Additional Resources

### Useful links

- Random Nerd Tutorials – ESP32 Web Server: Control an RGB LED https://randomnerdtutorials.com/esp32-rgb-led-web-server/

- Espressif docs – LEDC PWM Controller overview https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/ledc.html

- Arduino example – WiFi SimpleWebServer (bundled with ESP32 core)
