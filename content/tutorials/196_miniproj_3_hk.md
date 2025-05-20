## Distance Finder:

In this guide, we will learn how to use the ESP32 devboard, the Arduino IDE and the HC-SR04 ultrasonic sensor to make an easy and (pretty) accurate distance sensor.

This guide is split into 3 main parts:
1. Getting the Arduino IDE set up for your dev board
2. Assembling your circuit
3. Coding your distance finder

For this guide, you will need:
1. Your dev board
2. Your HC-SR04 ultrasound sensor
3. A breadboard
4. Jumper wires 
The Arduino IDE, which you can find [here](https://www.arduino.cc/en/software/ "Arduino downloads page")

**Note**: _Before starting, ensure that you have your dev board set up and the Arduino IDE is able to recognize and upload code to it. You can do this by connecting your dev board to your computer when it's OFF, and turn the switch on ONLY once connected. You can also hold down the BOOT button._

### Configuring the Arduino IDE for your ESP32:

**Note**: _You may have this working already from a previous mini-project. If this is the case, feel free to skip ahead!_

1. Make sure you have the ESP32 library installed. You can do this by:
	a. Click on the "libraries" tab
	b. Search for "ESP32"
	c. Click "install" on the first option
2. 
.
.
.

![Preferences Window](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/Screenshot%202025-05-19%20225342.png "Preferences Window")
And now you can upload Arduino code to your dev board!

### Building the circuit:

1. Place your sensor wherever you see fit in your breadboard
2. Connect your sensor to your Arduino as follows:
	a. VCC to 5V
	b. GND to GND
	c. Trig to pin 11
	d. Echo to pin 12
	**Note**: _You can change which pins you use with Trig and Echo, just make sure to change the pins you're using in the code._
.
.
.

You are now ready to start coding!

### Coding your distance finder:

1. To start, you will need to let your Arduino know which pins do what. You can do this using `int`, like this:

```
int trigPin = 11;    
int echoPin = 12;    
```
.
.
.

The HC-SR04 is a very well documented sensor, the internet is your friend if you want to learn more!