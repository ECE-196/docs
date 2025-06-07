## Current/Voltage Sensor Turorial:

Welcome to the Adafruit INA219 Current/Voltage Sensor tutorial where we will be using this sensor to measure the current, votlage, and power of our circuit. This was very useful for our project in ECE 196 when wanting to display our data so we know how well our solar charger is performing. However for this tutorial we will be measuring these things in a much simpler LED circuit.

## The Circuit:
- Breadboard
- Adafruit INA219
- Arduino Uno Mini
- An LED
- A Resistor I will be using a 1k Ohm resistor, but anything will work (the smaller the brighter)
- 6 female to male wires
- 4 male to male wires

Here's what my circuit looks like with everything connected:
![image](https://github.com/user-attachments/assets/f037e026-4da9-4135-97ab-38cb2e83557c)

I know it looks messy and is hard to follow so here's how to connect everything:
- First take your Arduino Uno Mini and plug in 2 male to male wires into the following ports (5V and GND. Next plug in your 5V and GND wires into your breadboard.
- Then take 2 male to female wires and plug those into ports A4 and A5 on your Arduino Uno Mini. Then proceed to plug those into SDA and SCL on your Adafruit INA219.
- Next put your LED and resistor on your breadboard and make sure the resistor is connected to the long part of the LED.
- Connect a wire from the short part of the LED to GND and the node of the resistor (not connected to the LED) to D7 on your Arduino.
- Next you will take 2 more male to female wires and connect them from your 5V and GND nodes on your breadboard to your VCC and GND ports on your INA219.
- Lastly you will take your last 2 male to female wires and connect one of them to Vin- and the node connected to D7 and the other wire from Vin+ to the node connected to your 5V.

## Coding:
- Firstly you will need to download Arduino IDE.
- Once you've done that go to your library manager and download Adafruit INA219.
- Then connect your Arduino into your computer and go to tools --> Board --> Arduino AVR Boards --> Arduino Uno Mini.
- After that go back to tools and click the COM connected to your Arduino.

You will then proceed to type in the following code to ensure your LED is on and to measure your bus voltage, shunt voltage, current, and power:
![Screenshot 2025-06-06 225059](https://github.com/user-attachments/assets/705b5f3d-0715-4b24-8648-e0aa6f8b7f02)
![Screenshot 2025-06-06 225110](https://github.com/user-attachments/assets/d7b0a9ce-8729-42ad-80b7-094f0c9afbcd)

These were the readings I was getting from the serial monitor (yours should be pretty similar):

![Screenshot 2025-06-06 224644](https://github.com/user-attachments/assets/053cdc93-084b-429c-b295-281a86d25cc8)

## Things to look out for:
- Once you finish building the circuit make sure the LED is on!! If the LED is not on then you have no current going through your circuit.
- When setting up your Arduino IDE make sure you're connected to the correct COM otherwise your code will fail to upload.
- You also want to make sure that the number in your serial begin matches the the number in the bottom right of your serial monitor.
  ![image](https://github.com/user-attachments/assets/c14c9904-16e3-4b41-81c9-7b858ec35f1a)
  ![image](https://github.com/user-attachments/assets/fa6f3f9e-77c3-4784-beeb-2abc26bc2d1e)

## Additional Applications:

In addition to power monitoring for your LED circuit, your solar charger or whatever circuit you have. The INA219 Current/Voltage Sensor is also good for detecting overcurrent or overvoltage in Circuit Breakers or EV charging systems. It may also come in handy when detecting energy loss in a system or if you want to adjust your current/voltage when managing speed control.

I hope this tutorial helped you understand how to use and apply an INA219 Sensor!!
