---
title: Leaderboard and Flash ESP32 LED after Successful Upload
date: 2025-06-06
authors:
  - name: Ghsialin Demeester
---

![LED flashing](Team17Photos/ledFlashing.jpg)

## Introduction

This tutorial will teach you how to build a **live leaderboard** and get your ESP32's led to flash once you've updated it! By the end of this tutorial you will:

1. Build a simple web page that displays people's standings in real - time.
2. Program an ESP32 to **flash an on-board LED** each time the POST succeeds.
   
### Learning Objectives

- Web development(dealing with result requests)
- ESP32, how to update leaderboard
- Toggle LED flash when leaderboard is updated
  


### Background Information

This leaderboard can keep track of the scores of you and your friends. 

A leaderboard is essentially a **key/value database** (user → points) exposed via a REST interface.  
We’ll implement two endpoints:

| Action | HTTP Verb | Path      | Body / Params             | Purpose                    |
|--------|-----------|-----------|---------------------------|----------------------------|
| Read   | `GET`     | `/scores` | –                         | Return full standings      |
| Write  | `POST`    | `/scores` | `{ "user": "...", "pts": n }` | Add or update a user score |




## Getting Started

Three pieces are required:
1. **Computer** to host the API and front-end capable of running python and Arduino IDE
2. **ESP32** with an LED either on-board or external
3. **Network** this tutorial was tested with mobile hotspot

### Required Downloads and Installations  

| Tool / Library              | Purpose                         | Install Guide |
|-----------------------------|---------------------------------|---------------|
| **Python ≥ 3.10**           | Run FastAPI server              | <https://python.org> |
| **FastAPI & Uvicorn**       | REST framework / ASGI host      | `pip install fastapi uvicorn` |
| **Arduino IDE (or VS Code + PlatformIO)** | Flash ESP32 | <https://arduino.cc/en/software> |

### Required Components  

| Component Name            | Quantity |
|---------------------------|----------|
| ESP32 DevKit (S2/S3/C3)   | 1 |
| USB cable (data-capable)  | 1 |
| Breadboard LED (optional) | 1 |
| 220 Ω resistor (if external LED) | 1 |
| Jumper wires              | 2–3 |




## Part 01: Back-End (FastAPI)

### Introduction

I wanted to make a simple leaderboard tutorial because it is quite easy to make and can give your project a really user-friendly and competitive aspect.

### Objective

- Create two enpoints (/scores GET & POST)
- Serve the future front-end(HTML/JS) from the same app

### Background Information

- FastAPI is a Python web framework
- Uvicorn is the Asynchronous Server Gateway Interface(ASGI) that listens on the port.
- Data will live in a simple Python dictionary (scores = {}) so you can stay laser-focused on the HTTP flow.


### Components

- Python >= 3.10
- pip install fastapi uvicorn
- VS Code

### Instructional

Teach the contents of this section
Create a folder 
`mkdir leaderboard`
`cd leaderboard `
Make a main.py and paste this code :

```
from fastapi import FastAPI, HTTPException
from fastapi.staticfiles import StaticFiles
from pydantic import BaseModel
import uvicorn

app = FastAPI(title="Live Leaderboard")
scores = {}                    # in-memory {user: pts}

class Entry(BaseModel):
    user: str
    pts:  int

@app.get("/scores")
def all_scores():
    return sorted(scores.items(), key=lambda kv: kv[1], reverse=True)

@app.post("/scores", status_code=201)
def add_score(e: Entry):
    if not e.user:
        raise HTTPException(400, "User cannot be empty")
    scores[e.user] = e.pts
    return {"ok": True}

# serve future index.html
app.mount("/", StaticFiles(directory=".", html=True), name="static")

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)

```

### You can then run the server
`python main.py`

Terminal should show: Uvicorn running on http://127.0.0.1:8000



## Part 02: Front-End (HTML + JavaScript)

### Introduction  

With the back-end running, you need a quick way to **see** those scores update.  
In this section you’ll build a single-file front-end (`index.html`) that:

1. Fetches `/scores` every couple of seconds.  
2. Re-renders a table with the latest standings.  

No frameworks—just plain HTML, a dash of CSS, and the browser’s native `fetch()`.

### Objectives  

- Load a static file from FastAPI’s `StaticFiles` mount.  
- Use `fetch()` to call a JSON endpoint.  
- Dynamically update the DOM without reloading the page.  

### Background Information  

- Modern browsers support **Fetch API**—a promise-based replacement for `XMLHttpRequest`.  
- A simple `setInterval()` loop is enough for “live-ish” updates (every 2 s in this tutorial).  
- The table will always render top-to-bottom, highest score first, because the back-end already sorts the list.

### Components  

| Item                     | Purpose                    |
|--------------------------|----------------------------|
| `index.html` (new file)  | front-end UI               |
| Browser (Chrome, Edge…)  | runs the page              |
| *(No extra hardware)*    |                            |

> **Tip**: Keep `index.html` in the same folder as `main.py` so FastAPI can serve it automatically.

### Instructional  

1. **Create `index.html`** in the same `leaderboard/` directory.  
2. Paste the 20-line starter below:

```html
<!doctype html>
<meta charset="utf-8">
<title>Live Leaderboard</title>
<style>
body{font-family:system-ui,sans-serif;max-width:24rem;margin:auto}
h2{text-align:center}
table{width:100%;border-collapse:collapse}
th,td{padding:.4rem .6rem;border-bottom:1px solid #ccc;text-align:left}
</style>

<h2>🏆 Leaderboard</h2>

<table id="board">
  <thead><tr><th>User<th>Points</tr></thead>
  <tbody></tbody>
</table>

<script>
const tbody = document.querySelector('#board tbody');

async function refresh(){
  try{
    const res  = await fetch('/scores');
    const data = await res.json();          // [[user, pts], …]
    tbody.innerHTML = data
      .map(([u,p]) => `<tr><td>${u}<td>${p}`)
      .join('');
  }catch(err){ console.error('Fetch failed', err); }
}

refresh();                // run once on load
setInterval(refresh,2000); // repeat every 2 s
</script>
```
Reload `http://localhost:8000/` in your browser.

You should see an empty table.

## Part 03: ESP32 Client (Wi-Fi JSON & LED Flash)

### Introduction  

Your web stack is live—now let’s feed it with *real* hardware data.  
In this section you’ll program an ESP32 to:

1. Connect to your Wi-Fi network.  
2. POST its latest score to `/scores`.  
3. Blink the on-board LED **once** as a quick “upload succeeded” confirmation.

### Objectives  

- Configure `HTTPClient` for a simple JSON POST.  
- Build a tiny helper that blinks a GPIO for ~120 ms.  
- See the browser table update automatically after each upload.

### Background Information  

- **HTTPClient** (bundled with Arduino-ESP32) is perfect for one-off REST calls.  
- A blocking `delay(120)` blink is fine, because the sketch is already waiting for the HTTP response.  
- If your board’s LED isn’t on GPIO 2, just change the `LED_PIN` constant or wire an external LED + 220 Ω to any spare pin.

### Components  

| Part          | Qty | Notes                                   |
|---------------|-----|-----------------------------------------|
| ESP32 dev-kit | 1   | S2 / S3 / C3—all work                   |
| USB data cable| 1   | Flashing & Serial Monitor               |
| (Optional) LED + 220 Ω | 1 | Only if your board has no built-in LED |

### Instructional  

1. **Open Arduino IDE** → *File → New Sketch*.  
2. Paste the **snippet** below and edit three items:  
   - `WIFI_SSID`  
   - `WIFI_PASS`  
   - `<PC_IP>` (your computer’s LAN IP, e.g. `192.168.4.12`)

```c
#include <WiFi.h>
#include <HTTPClient.h>

/* Wi-Fi credentials ------------------------------------------------ */
const char* WIFI_SSID = "YOUR_WIFI";
const char* WIFI_PASS = "YOUR_PASS";

/* FastAPI endpoint ------------------------------------------------- */
const char* SERVER_URL = "http://<PC_IP>:8000/scores";   // ← change IP

/* LED pin ---------------------------------------------------------- */
const uint8_t LED_PIN = 2;   // built-in LED on most dev-kits

/* Helper: connect Wi-Fi ------------------------------------------- */
void wifi_connect() {
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED) delay(250);
}

/* Helper: 120 ms blink -------------------------------------------- */
inline void flashLED(uint16_t ms = 120) {
  digitalWrite(LED_PIN, HIGH);
  delay(ms);
  digitalWrite(LED_PIN, LOW);
}

/* Helper: send JSON score ----------------------------------------- */
void sendScore(const String& user, uint32_t pts) {
  if (WiFi.status() != WL_CONNECTED) wifi_connect();

  HTTPClient http;
  http.begin(SERVER_URL);
  http.addHeader("Content-Type", "application/json");

  String body = "{\"user\":\"" + user + "\",\"pts\":" + pts + "}";
  int code = http.POST(body);

  if (code > 0) flashLED();   // blink on success
  http.end();
}

/* Arduino entry points -------------------------------------------- */
void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  wifi_connect();
}

void loop() {
  static uint32_t total = 0;
  total += random(15, 45);     // fake “points” for demo
  sendScore("name", total);
  delay(5000);                 // push every 5 s
}
```

### Analysis

This end-to-end demo proves the whole pipeline works: every time the ESP32 HTTP-POSTs a score, the LED flashes, FastAPI stores the value, and the browser’s fetch() call.


### Useful links
https://fastapi.tiangolo.com/
https://www.uvicorn.org/
https://github.com/espressif/arduino-esp32/tree/master/libraries/HTTPClient
List any sources you used, documentation, helpful examples, similar projects etc.
