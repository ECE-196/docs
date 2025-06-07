---
title: ALERT LED
date: 06-06-2025
#authors
  - name: Yousef Alkhunaizi
---

![Motion Detection Setup](Team5/ESP32.png)

## Introduction

This tutorial will show how to light up LEDs connected to an ESP32 when abnormal motion is detected by an accelerometer.

### Learning Objectives

-  Understand how to read and interpret accelerometer data using CircuitPython.
- Detect sudden motion or acceleration beyond a threshold.
- Trigger LEDs to respond to abnormal movement.

### Background Information

- Accelerometers are sensors that measure acceleration forces. These forces may be static, like the constant force of gravity, or dynamic — caused by moving or vibrating the sensor.

- In this project, we will detect sudden changes in acceleration (abnormal motion) and respond by turning on LEDs.


## Materials Required
- ESP32 Envision DevBoard  
- MPU6050 (or any 3-axis accelerometer)  
- Breadboard and jumper wires  
- 1 or more LEDs

### Required Downloads and Installations

Before you begin, you'll need to install a few things to program the ESP32 and interact with the accelerometer:

---

#### 1. Install CircuitPython on Your ESP32 DevBoard  
Follow Adafruit’s guide here:  
[https://learn.adafruit.com/circuitpython-on-the-esp32-s2](https://learn.adafruit.com/circuitpython-on-the-esp32-s2)

Make sure you:
- Use a **Chromium-based browser** (like Chrome or Edge)
- Flash the `.uf2` firmware onto the board

---

#### 2. Install the CircuitPython Library Bundle  
You need libraries like `adafruit_mpu6050`.

- Download the bundle from:  
  [https://circuitpython.org/libraries](https://circuitpython.org/libraries)
- Unzip the bundle
- Copy the following into the `lib/` folder on your ESP32:
  - `adafruit_mpu6050.mpy`
  - `adafruit_bus_device` (folder)

---

#### 3. Install Visual Studio Code  
Download and install from:  
[https://code.visualstudio.com/](https://code.visualstudio.com/)

- Then install the **Python** and **Pymakr** extensions (optional but helpful).

---
#### Pros:
- Useful for fall detection and alert systems.
- Easy to implement with CircuitPython.
- Compatible with low-cost components.

#### Cons:
- Sensitive to orientation and noise.
- Requires tuning threshold values for each use case.

---

## Materials Required

| Component             | Quantity |
|-----------------------|----------|
| ESP32 DevBoard        | 1        |
| MPU6050 Accelerometer | 1        |
| LED                   | 1        |
| 220Ω Resistor         | 1        |
| Jumper Wires          | 5        |
| Breadboard            | 1        |

---
## Circuit Setup

1. Connect MPU6050:
   - VCC → 3.3V  
   - GND → GND  
   - SDA → GPIO21  
   - SCL → GPIO22  

2. Connect LED:
   - Anode (long leg) → GPIO5 (through 220Ω resistor)  
   - Cathode (short leg) → GND  

---

## Code (CircuitPython)
# %%
```python
import time
import board
import busio
import digitalio
from adafruit_mpu6050 import MPU6050

# Setup I2C and accelerometer
i2c = busio.I2C(board.SCL, board.SDA)
mpu = MPU6050(i2c)

# Setup LED
led = digitalio.DigitalInOut(board.IO5)
led.direction = digitalio.Direction.OUTPUT

# Threshold for abnormal motion (adjust based on tests)
threshold = 18

while True:
    x, y, z = mpu.acceleration
    total = abs(x) + abs(y) + abs(z)

    if total > threshold:
        led.value = True
    else:
        led.value = False

    time.sleep(0.1)





