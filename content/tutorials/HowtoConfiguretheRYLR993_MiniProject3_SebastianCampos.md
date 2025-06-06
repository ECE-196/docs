---
title: How to set up RYLR993 LoRa Modules for Basic Communication
date: 2025-6-5
authors:
  - name: Sebastian Campos
---

![alt text](image.png)

## Introduction

This tutorial is going to teach readers how to setup two RYLR993 modules for basic communication. 
I decided to create this tutorial because it was tedious to figure out how to configure the modules as the online videos
available were often unclear. Overall, readers should be able to understand the basic wiring and serial commands needed to set up
the modules.


### Learning Objectives

- Learn how to wire the LoRa modules to recieve and transmitt signals
- Learn the serial commands needed to configure the LoRa modules
- Understand what hardware is needed to setup the modules
- Write simple signal reception code

### Background Information

![alt text](image-1.png)

LoRa is short for "long range" and is a form of communication that is able to send 
signals over long distances using low power. LoRa uses a chirped modulation
format to send its signals and is a wide-spectrum technology. LoRa can be used in many applications
but is often used for direct line-of-sight wireless communication in IoT (internet of things)
devices.

Sigfox is another form if communcation that is used in IoT devices which 
uses binary phase-shift keying. Generally, Sigfox communication is less noisy than LoRa because it's 
receivers only listen to a small signal spectrum. However, Sigfox basestations 
are more expensive compared to LoRa. Furthermore, Sigfox is not available everywhere and users can not
manage their own network. Finally, LoRa is able to send signals bidirectionally due to its symmetric link
technology. Although Sigfox can be configured to work bidirectionally, there are more requirements as
Sigfox is an inherently asymetric link. 

All things considered, LoRa is a good option for IoT devices
because it is supported in the US, is cheap to use, and uses low power.

## Getting Started

### Required Downloads and Installations

To complete this tutorial, download Arduino version 1.8.16 by completing the following steps:

1. Go to https://www.arduino.cc/en/software/OldSoftwareReleases/
2. Click and Install the version for your operating system by following the built-in guide
3. Nice! You have downloaded all the software needed to complete this tutorial
4. Note: Version 1.8.16 is used because it includes an easy to use serial monitor

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| RYLR993        |    2      |
|                |           |
| Male to Female |    8      |
| Jumper cables  |           |
|                |           |
| CP2102 USB to  |    1      |
| TTL 5PIN Serial|           |
| Converter for  |           |
| UART STC 3.3V  |           |

### Required Tools and Equipment

- Computer
- ESP 32
- USB-C to USB-A adapter (Only if your device does not have a USB-A port)
- Another computer or power supply (needed to power the ESP 32 while the CP2102 is connected to the computer)
- Note: the CP2102 will be connected to your computer using a cable. I used an old CP2102 which uses a USB-A to USB-Mini. However, newer modules may use USB-A to USB-C or USB-C to USB-C.

## Part 01: Connecting the CP2102 to the RYLR998 and RYLR993

### Introduction

In this section you will learn how to wire the CP2102 and RYLR993 modules together. 

### Objective

- Understand how to wire the CP2102 to the RYLR993 modules.

### Background Information

To complete this section, you just need to make sure your
jumper cables are properly connecting each module.

### Components

- 1 CP2102
- 2 RYLR993 (should include coax and monopole antenna)
- 4 Jumper Wires
- 1 USB-A to USB-Mini cable (your cable may be different)

### Instructional

1. Connect the **GND** male pin on the CP2102 to the **GND** female pin of the RYLR993 using a male to female (M-to-F) jumper cable.
2. Connect the **3.3V** male pin on the CP2102 to the **3.3V** female pin of the RYLR993 using a M-to-F jumper cable.
3. Connect the **Tx** male pin on the CP2102 to the **Rx** female pin of the RYLR993 using a M-to-F jumper cable.
4. Connect the **Rx** male pin on the CP2102 to the **Tx** female pin of the RYLR993 using a M-to-F jumper cable.
5. Connect the CP2102 USB-A cable to your computer

- Note: Rx goes to Tx and Tx goes to Rx is correct and necessary for proper configuration.

## Visual

![alt text](D7F6E4B5-0C94-4F49-9E06-DA7F1CFBF00D_4_5005_c.jpeg)

## Part 02: Configuring the RYLR998

### Introduction

Now that every module is properly connected, you can now learn
what commands are necessary to configure the RYLR998.

### Objective

- Understand the commands needed to configure the RYLR998

### Background Information

To complete this section, you will need to open your arduino IDE
and use the serial monitor to configure the RYLR993 through the 
CP2102. The commands used in this tutorial are taken directly from the 
[documentation.](https://reyax.com/upload/products_download/download_file/RYLR993_AT_Command.pdf) 
You can open the 
[documentation](https://reyax.com/upload/products_download/download_file/RYLR993_AT_Command.pdf)
to verify the commands being used here. 

The beauty of using the CP2102 is that you can simply plug and play. The CP2102 does not need to be configured like the 
RYLR998. 

### Components

- No other components needed

### Instructional

1. Open the Arduino IDE
2. Go to Tools -> Port and make sure the CP2102 port is connected. On
My side the following is displayed: /dev/cu.usbserial-A900fshd. Your port name may be slightly different, but the correct choice should be fairly straigthforward.
3. Go to Tools -> Serial Monitor to open the serial monitor
4. In the serial monitor, follow the next steps: 
  - Type `AT+OPMODE=?` and press "Return" on your keyboard (from now on, everytime you type a command you must press enter or return)
  - The serial monitor should respond with `0` and then `OK`
  - `+OK` response indicates that your wiring is correct and you can proceed with configuring the RYLR998. 
  - If the serial monitor responds with `AT_ERROR`, then you most likely have wired your modules incorrectly or have typed the command incorrectly. You must fix this before proceeding.
5. Now, `0` indicates that you are on the LoRaWan mode. You will need to change the mode to the REYAX RYLR998 proprietary mode following the next steps:
  - Type `AT+OPMODE=1`
  - The serial monitor will with `OK`and then `Need RESET!`
  - Reset the 993 by clicking the button on the module, located behind the chip
  - The serial monitor will respond with `+READY`
  - Now we are on opmode 1, we can use the RYLR998 commands even though we are using a 993.
  - In the next steps we will need to set the frequency and ADDRESS ID
6. By default the frequency should be at 915 Mhz. However, to make sure the frequency is correct, complete the following steps
  - Type `AT+BAND=?`
  - The serial monitor should respond with `+BAND=915000000` and then `OK`
  - If for some reason the band is incorrect type the following `AT+BAND=915000000` and then press the reset button
7. Now we must setup the address ID. By default, the addresses are set to 0. You can keep the addresses at 0 or change each LoRa module to have its unique address.
  - Set the address by typing `AT+ADDRESS=2` (note you can choose any two unique addresses from 0 - 65535 for your modules) and then pressing the reset button
  - Check that the setup worked by typing `AT+ADDRESS=?`
8. Now one of your RYLR998 modules is setup properly. Now repeat the steps for your other RYLR993 module.

- Note: There are many other commands to configure the RYLR998 modules. However, this tutorial is meant to illustrate only the essential configs for basic one-to-one communication. For instance, if a RYLR998 is used instead, you can set the NETWORK ID from 0 - 18; However, the RYLR993 only supports one default network ID = 18.

### Visual

![alt text](image-2.png)

## Part 03: Writing Simple LoRa Reception Code

### Introduction

In this section you will learn how to write simple reception code for your ESP 32. Another benefit of the CP2102 is that it can also send signals to other LoRa modules. We will be illustrating signal reception success by writing reception code using Arduino.

### Objective

- Understand how to send signals using the CP2102
- Understand how to write simple LoRa reception code
- Learn how to upload Arduino code to an ESP 32

### Background Information

To complete this section, you will need to understand basic programming logic. The most "confusing" parts of this section mainly come from the setup commands. However, to complete this tutorial, these commands are not necessary to fully understand.

### Components

- 1 ESP 32
- 2 RYLR993
- 8 Jumper Wires
- Power bank or another computer

### Instructional

1. Similar to the wiring setup used to connect the CP2012 and RYLR993 (3.3V to 3.3V, GND to GND, ... etc), now connect the remaining RYLR993 to your ESP 32. And connect the ESP 32 to your computer
- Note: you should still have one of your RYLR993's connected to the CP2012 (not connected to your computer)
2. Now, before you write the code, you want to make sure your ESP 32 is properly connected to your computer by going to Tools -> Ports
- The port should be named as follows: /dev/cu.usbmodem101 (ESP32 Family Device)
3. Now that your ESP 32 is connected to your board, you can write the following code into your Arduino IDE:

```
const unsigned int LED{17}; // define a constant for the LED pin
String incomingstring;

void setup() {
    pinMode(LED, OUTPUT); // configure the LED pin to be an output
    Serial.begin(9600);
}

void loop() {
    if(Serial.available()) {
        incomingstring = Serial.readString();
        if(incomingstring.indexOf("H") > 0) {
            digitalWrite(LED, HIGH);
        } else if (incomingstring.indexOf("L") > 0) {
            digitalWrite(LED, LOW);
        }

    }
}
```
This code does the following:
- Defines a constant for the LED pin. In your ESP 32, your can use PIN 17 or 18
- Creates a string variable which represents the incoming signal information
- In setup(), the LED is configured and the baud rate is set to 9600
- In loop(), serial.available() is used to check if a signal is detected. If it is, then it will store the signal as a string into incoming string
- Then, if the signal sent has a "H" in the signal, then the LED will turn ON
- If, the signal sent has a "L" in the signal, then the LED will turn OFF

Essentially, this code makes the ESP 32 ready for any sent signals. When a signal is received it will turn an LED ON or OFF

4. Now, unplug the ESP 32 and connect it to your other computer or power supply and make sure it turns on. Connect your CP2102 (which is connected to your other RYLR993) to your computer. Now we can send a signal and see if your ESP 32 reciever can detect the signal
5. Open up the serial monitor (no need to change into a new Arduino sketch) and follow the next steps
- Note: I configured both 993's to have an ADDRESS = 2. 
- Type `AT+SEND=2,1,H`. The first number in this command is the address of the module you are sending your signal too, the 1 represents the length of the string, and H represents the character in the string
- Change the address number to the number you chose in step 2.
- The serial monitor should respond with `+OK` if your signal was sent properly. If not, then an error will show as aforementioned. Check the documentation. to see what your error is and try to debug.
- If everything was setup properly, the LED on your ESP 32 should have turned ON and stayed ON
- Now you can type `AT+SEND=2,1,L` to turn OFF the LED
- You have officially finished configuring the 993 modules!

## Example

![alt text](72BCA896-0FFD-41C5-AEC2-9499954CE79D_4_5005_c.jpeg)

### Analysis


Overall, this example uses a CP2102, RYLR993 modules, and an ESP 32 to send basic signals. The LoRa modules where configured using the CP2102 and the appropriate commands to set the opmode, frequency, and address IDs. The receiver code was uploaded onto the ESP 32 shown, and a signal was sent to it using the CP2102 connected to the computer. The signal was sent using the `AT+SEND` command through the CP2102. The reception code was programmed to set the string character 'H' as high and 'L' as low, or LED ON and LED OFF, respectively. After a signal was sent to turn ON the LED, the LED on the ESP 32 should turn ON. As shown in the image above, the ESP 32 LED turns ON red, indicating that the modules were correctly configured and the reception code works.

## Additional Resources and Useful Links

- [RYLR993 Documentation](https://reyax.com/upload/products_download/download_file/RYLR993_AT_Command.pdf)
- [RYLR998 Documentation](https://reyax.com/upload/products_download/download_file/RYLR998_EN.pdf)
