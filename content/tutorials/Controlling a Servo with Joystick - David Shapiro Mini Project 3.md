# Title: Arduino Mega 2560 Servo Control with Joystick Tutorial
date: 2025-06-06
authors:
  - name: David Shapiro

Note: While this tutorial uses the Arduino Mega 2560 (as my ESP32 was being used for the final project), the principles remain very similar. ESP32 code is provided at the end for reference.

## Learning Objectives

- Understanding basic servo motor control
- Reading analog input from a joystick module
- Programming the Arduino Mega for motor control applications
- Working with Arduino libraries

## Required Components

- 1x Arduino Mega 2560 board
- 1x Servo motor (SG90 or similar)
- 1x Analog joystick module
- Jumper wires
- Breadboard
- USB cable for programming

## Required Libraries

In the Arduino IDE, you only need the standard Servo library (included with Arduino IDE)

## Circuit Connections

Connect the components as follows:

### Joystick Module:

- VCC → 5V
- GND → GND
- VRx (X-axis) → A1

### Servo Motor:

- Red wire (Power) → 5V
- Brown/Black wire (Ground) → GND
- Orange/Yellow wire (Signal) → Pin 50

## Arduino Mega Code

```
#include <Servo.h>

// Create servo object
Servo myservo;

// Define pins
const int joystickXPin = A1;  // Joystick X-axis
const int servoPin = 50;      // Servo control pin

// Variables
int joystickValue;
int servoAngle;

void setup() {
  // Initialize serial communication
  Serial.begin(9600);
  
  // Attach servo to pin
  myservo.attach(servoPin);
}

void loop() {
  // Read joystick value (0-1023)
  joystickValue = analogRead(joystickXPin);
  
  // Map joystick value to servo angle (0-180)
  servoAngle = map(joystickValue, 0, 1023, 0, 180);
  
  // Control servo
  myservo.write(servoAngle);
  
  // Debug output
  Serial.print("Joystick Value: ");
  Serial.print(joystickValue);
  Serial.print(" Servo Angle: ");
  Serial.println(servoAngle);
  
  // Small delay for stability
  delay(15);
}
```

## Important Notes for Arduino Mega

- The Arduino Mega's ADC has 10-bit resolution (0-1023 values)
- The standard Servo library is simpler to use than the ESP32 version
- 5V power is more stable for servos compared to ESP32's 3.3V logic

## Troubleshooting

- If movement is reversed, adjust the map() function values

## ESP32 Code(Not fully Tested)
-Slight modifications to the circuit must be made, Changing the pins for Joystick and Servo.
```
#include <ESP32Servo.h>

// Create servo object
Servo myservo;

// Define pins
const int joystickXPin = 1;  // Joystick X-axis
const int servoPin = 46;      // Servo control pin

// Variables
int joystickValue;
int servoAngle;

void setup() {
  // Initialize serial communication
  Serial.begin(115200);
  
  
  // Attach servo to pin
  myservo.attach(servoPin, 500, 2400);
}

void loop() {
  // Read joystick value (0-4095)
  joystickValue = analogRead(joystickXPin);
  
  // Map joystick value to servo angle (0-180)
  servoAngle = map(joystickValue, 0, 1023, 0, 180);
  
  // Control servo
  myservo.write(servoAngle);
  
  // Debug output
  Serial.print("Joystick Value: ");
  Serial.print(joystickValue);
  Serial.print(" Servo Angle: ");
  Serial.println(servoAngle);
  
  // Small delay for stability
  delay(15);
}
```
