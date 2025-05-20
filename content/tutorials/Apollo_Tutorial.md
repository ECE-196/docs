Hello! In this tutorial, we will be using the Adafruit INA219, a current and voltage sensor, useful for calculating power and detailing information in your circuit. This guide will be a simple step-by-step guide to integratting it into your next project using the ESP32 Devboard!

For now, we will start simple by simply measuring the current and voltage across an LED in an LED circuit (don't forget the resistor).

## Materials You Will Need

- Adafruit INA219
- ESP32 Devboard (or similiar counterpart, like an Arduino)
- USB C cable
- Arduino IDE
- Breadboard (optional, but very convenient)
- An LED (color is up to you)
- A resistor (I will be using 10k Ω)
- Wires (minimum amount):
    - 6 Female-to-male wires
    - At least 3 breadboard jumper wires


## Wiring
Here is a picture of everything put together.

![my image](./IMG_2875.JPG)

Below is a step-by-step guide to wiring the circuit:
1. Lay out your parts, making sure you have all the materials needed.
2. Wire your circuit, but keep in mind the following:
    - Whether 5V or 3.3V, the INA219 can use both. Of course, be aware of how this might affect your project if you use the 3.3V pin
    - SDA/SDL are signal pins. For the ESP32, you can put it in any of the GPIOs (general pin input/outputs), but for Arduino, it is much more specific. For example, on an Arduino UNO, you HAVE to put the SCL in A4 and SDA in A5.
    - VCC -> same pin as 5V 
    - GND -> GND pin
    - The most confusing part: Vin+ and Vin-. PLEASE note that these are NOT the same as VCC and GND. This is where the INA219 will be measuring... put your Vin- at the anode (postive leg) of the LED, and the Vin+ pin into your same pin as VCC. If done correctly, your LED will light up. If done incorrectly, you won't be able to receive information. If you put them in reverese, you are expected to get negative voltage.

Great, you now wired your circuit. Let's move on to the software aspect of this tutorial.

## Software
Using Arduino C, we will be able to read the current and voltage. Again, it is crucial if your LED light is on, as this means current is running through the circuit. Do not move on until you know your circuit has current running.

1. Open your Arduino IDE
2. Make sure you have the IDA219 by Adafruit library installed.
3. With the library installed, go to File -> Examples -> Adafruit INA219 -> get_current.ino.
4. Upload the code.
    NOTE: It's possible the default baud rate is very high. Change it to your usual baud rate (I presume 9600).
    Additionally, make sure your CDC is enabled for the ESP32, as it isn't automactially available. This is required to read data.
    Arduino doesn't need to do this.

Now, watch your circuit read data. :)

![my image](./IMG_2868.JPG)

