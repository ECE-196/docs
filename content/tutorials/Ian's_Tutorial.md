title: Arduino

Photoresistor Circuit Tutorial

date: 2025-06-06
author:
  - name: Ian Cunningham

In this tutorial, you will learn how to create a simple light-detecting circuit using a photoresistor (LDR) and an Arduino Uno Mini. The circuit will allow your Arduino to detect light in its surroundings.

## Components Needed

- Arduino Uno Mini (any Arduino board will work)
- Photoresistor (LDR)
- 10kΩ resistor
- Breadboard
- Jumper wires

## Circuit Diagram


![5V](https://github.com/user-attachments/assets/9dab49e0-b71d-436f-8d13-933a10ff388f)



## Step-by-Step Instructions

### 1. Connect all of the Components

1. Connect one leg of the photoresistor to the 5V pin of the Arduino.
2. Connect the other leg of the photoresistor to:
   - Analog pin A0 of the Arduino
   - One leg of the 10kΩ resistor
3. Connect the other leg of the 10kΩ resistor to the GND pin of the Arduino.

![Photoresistor](https://github.com/user-attachments/assets/44480828-628e-4d61-83bb-4e47dc0be576)

This creates a voltage divider circuit where the voltage at A0 will change based on the light level.

### 2. Understanding the Circuit

The photoresistor changes its resistance based on light intensity:
- Bright light = lower resistance (about 1kΩ)
- Darkness = higher resistance (can be several hundred kΩ)

The 10kΩ resistor forms a voltage divider with the photoresistor:
- In bright light, the voltage at A0 will be higher (closer to 5V)
- In darkness, the voltage at A0 will be lower (closer to 0V)

### 3. Upload the Arduino Code

```arduino
// Define the analog pin for the photoresistor
const int ldrPin = A0;

void setup() {

// Start serial communication
  Serial.begin(9600);
}

void loop() {
  // Read the photoresistor's analog value
  int ldrValue = analogRead(ldrPin);
  
  // Print the value to the serial monitor
  Serial.print("Light level: ");
  Serial.println(ldrValue);
  
  // Wait for a short while before reading again
  delay(500);
}
```

### 4. Test the Circuit

1. Upload the code to your Arduino.
2. Open the Serial Monitor (Tools > Serial Monitor).
3. Observe the values changing as you cover the photoresistor or expose it to light.
4. Approximate values:
   - Complete darkness: 0-100
   - Dim room: 300-500
   - Bright light: 800-1023

### 5. Calibration (Optional)

For more precise readings, you can calibrate your circuit:

```arduino
// Define your own thresholds based on your environment
const int darkThreshold = 200;

const int brightThreshold = 800;

void loop() {
  int ldrValue = analogRead(ldrPin);

  if (ldrValue < darkThreshold) {
    Serial.println("Dark");
  }
  else if (ldrValue > brightThreshold) {
    Serial.println("Bright");
  }
  else {
    Serial.println("Medium light");
  }

  delay(500);
}
```
Here is the output that the serial monitor displays when uploading the code to the arduino. The light value ranges from 0 - 1023.
![image](https://github.com/user-attachments/assets/03e509e4-56d4-417a-b270-82ad2000f122)

## Applications

This basic circuit can be used for:
- Automatic night lights
- Light-activated switches
- Plant monitoring systems
- Simple security systems (detect lights being turned on/off)

## Troubleshooting

1. **No change in values**:
   - Check connections
   - Make sure the photoresistor is not damaged (check resistance with a multimeter)

2. **Values stuck at 0 or 1023**:
   - Check for short circuits or open connections
   - Make sure resistor value is correct (10kΩ)

3. **Unstable readings**:
- Connect a small capacitor (0.1µF) between A0 and GND to filter out reading fluctuations
   - Check for loose connections

## Expanding the Circuit

You can combine this with other components like:
- An LED that will turn on when the light goes dark
- A relay to turn on/off lights automatically
- An LCD display to show light levels


![IMG_7298](https://github.com/user-attachments/assets/8cb16190-a2e5-4cc5-8f12-165be0fe3901)







