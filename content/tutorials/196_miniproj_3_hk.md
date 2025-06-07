## Distance Finder:

In this guide, we will learn how to use the ESP32 devboard, the Arduino IDE and the HC-SR04 ultrasonic sensor to make an easy and (really quite) accurate distance sensor.

![HC-SR04](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/177109536-2301652643.png "HC-SR04")

The HC-SR04 is an extremely popular ultrasonic sensor with a range of about 400cm, and a reasonably wide FOV (~30 degrees). The sensor is also accurate enough for most projects (0.1-0.5cm), which is why it is the sensor of choice for many projects that use distance or proximity readings. As such, becoming familiar with this sensor is advantageous, and can be very helpful for your final projects, or even just personal projects.

This guide is split into 3 main parts:
1. Getting the Arduino IDE set up for your dev board
2. Assembling your circuit
3. Coding your distance finder

For this guide, you will need:
1. Your dev board
2. Your HC-SR04 ultrasound sensor
3. A breadboard
4. Jumper wires 
5. The Arduino IDE, which you can find [here](https://www.arduino.cc/en/software/ "Arduino downloads page")

**Note**: _Before starting, ensure that you have your dev board set up and the Arduino IDE is able to recognize and upload code to it. You can do this by connecting your dev board to your computer when it's OFF, and turn the switch on ONLY once connected. You can also hold down the BOOT button._

### Configuring the Arduino IDE for your ESP32:

**Note**: _You may have this working already from a previous mini-project. If this is the case, feel free to skip ahead!_

1. Make sure you have the ESP32 library installed. You can do this by:\
	a. Click on the "libraries" tab on the toolbar on the left of the screen\
	b. Search for "ESP32"\
	c. Click "install" on the option shown below\
![Library Manager](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/Screenshot%202025-06-06%20170022.png "Library Manager")
2. Next, make sure the Arduino IDE knows where to look if it needs any more information about your board. This is not strictly necessary, but is highly recommended to prevent issues in the future.\
![Preferences Window](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/Screenshot%202025-05-19%20225342.png "Preferences Window")
3. Finally, tell the Arduino IDE what type of board you are interacting with. To do this click on Tools --> Board --> esp32 --> ESP32S3 Dev Module. You can also do this by simply clicking on the name of the board shown in the toolbar and clicking "Select other board and port...", where you can search for and select ESP32S3 Dev Module.\
![Board Selection](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/Screenshot%202025-06-06%20175813.png "Board Selection")
And now you can upload Arduino code to your dev board!

### Building the circuit:

1. Place your sensor wherever you see fit in your breadboard
![HC-SR04 in Breadboard](https://github.com/mjjhtbprof/ECE196_Hari_Kaushik/blob/main/content/tutorials/Screenshot%202025-06-06%20183037.png "HC-SR04 in Breadboard")   
2. Connect your sensor to your dev board as follows:\
	a. VCC to 5V\
	b. GND to GND\
	c. Trig to pin 11\
	d. Echo to pin 12\
	**Note**: _You can change which pins you use with Trig and Echo, just make sure to change the pins you're using in the code._

It's as simple as that. You are now ready to start coding!

### Coding your distance finder:

1. To start, you will need to let your Arduino know which pins do what. You can do this using `int`, like this:

```
int trigPin = 11;    // Trigger
int echoPin = 12;    // Echo 
```
2. Next, you need to set up the variables that will store all your data, you can do this using `long`:

```
long duration, cm, inches; //We use long because we are using extended variable sizes
```

3. Now, we add the code that we only need to run once, so this is added in the `setup` function. Here, we will start the serial monitor and tell the IDE which pin does what. For this, we use `Serial.begin` and `pinMode`:

```
  Serial.begin (9600);

 //Telling the IDE which pin does what
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
```
4. Next, we add the code that we want to repeat as long as the board is powered, and we do this by adding it to the `loop` function. Here, we need to trigger our ultrasound, listen for the return pulse, and calculate what distance that equates to. We do this using `digitalWrite`:

```
  // Trigger the sensor w/ a high of >=10us
  // Low pulse makes sure read is clean
  digitalWrite(trigPin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
 
  // Read the data from the sensor in the form of a high pulse
  // The duration of the pulse is the time it takes for the sound to travel to the object and back
  pinMode(echoPin, INPUT);
  duration = pulseIn(echoPin, HIGH);
 
  // Convert the time into a distance. This math takes into account the speed of sound. 
  // How can you recreate this if you just define the speed of sound earlier in the code?
  cm = (duration/2) / 29.1;     // Divide by 29.1 or multiply by 0.0343
  inches = (duration/2) / 74;   // Divide by 74 or multiply by 0.0135
  ```
If you're up for a challenge, try and answer the questions in the code comments!

5. It's not enough just to read and interpret values from the sensors. We also need to see them somehow. We can do this by printing the values in the serial monitor using `Serial.print`:

```
  //Print the values in in and cm
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  
```
Does this code go in the `loop` function?

6. Finally, we add a small delay so we don't have erratic values:

```
  // Delay so the readings aren't very erratic
  delay(250);
```
Make sure to upload this code to your dev board, and ensure that you have the correct COM port selected when you do so, you may encounter errors if you do not.

The HC-SR04 is a very well documented sensor, the internet is your friend if you want to learn more!

Congratulations! You have successfully completed the tutorial. Happy measuring!
