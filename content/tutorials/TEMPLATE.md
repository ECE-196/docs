---
title: How to set up RYLR998 and RYLR993 LoRa Modules for Basic Communication
date: 2025-19-25
authors:
  - name: Sebastian Campos
---

![relevant graphic or workshop logo](sebastiancampos/Downloads/LoRa_setup.png)

## Introduction

Write a short section on what the tutorial is aiming to accomplish.
What is the motivation behind the tutorial?
What do you want readers to gain from the tutorial?

This tutorial is going to teach readers how to setup RYLR998 and RYLR993 modules for basic communication. 
I decided to create this tutorial because it was tedious to configure the modules as the online videos
available were often unclear. Overall, readers should be able to understand the basic serial commands to set up
the modules.


### Learning Objectives

- Bullet list of skills/concepts to be covered

Any additional notes from the developers can be included here.
- Learn the serial commands needed to configure the LoRa modules
- Learn how to wire the LoRa modules to recieve and transmitt signals
- Understand what hardware is needed to setup the modules
- Write simple signal reception code


### Background Information

Describe your topic here. What does it do? Why do you use it?
Are there other similar things to use? What are the pros and cons?
Explain important concepts that are necessary to understand.
Include (and cite if needed) any visuals that will help the audience understand.

LoRa is short for "long range" and is a form of communication that is able to send 
signals over long distances using low power. LoRa uses a chirped modulation
format to send its signals and is a wide-spectrum technology. LoRa can be used in many applications
but is often used for direct line-of-sight wireless communication in IoT (internet of things)
devices. Nevertheless, Sigfox is another form if communcation that is used in IoT devices which 
uses binary phase-shift keying. Generally, Sigfox communication is less noisy than LoRa because it's 
receivers only listen to a small signal spectrum. However, Sigfox basestations 
are more expensive compared to LoRa. Furthermore, Sigfox is not available everywhere and you can not
manage your own network. Finally, LoRa is able to send signals bidirectionally due to its symmetric link
technology. Although Sigfox can be configured to work bidirectionally, there are more requirements as
Sigfox is an inherently asymetric link. All things, considered, LoRa is a good option for IoT devices
because it its supported in the US, is cheap to use, and its low power.

## Getting Started

For any software prerequisites, write a simple excerpt on each
technology the participant will be expecting to download and install.
Aim to demystify the technologies being used and explain any design
decisions that were taken. Walk through the installation processes
in detail. Be aware of any operating system differences.
For hardware prerequisites, list all the necessary components that
the participant will receive. A table showing component names and
quantities should suffice. Link any reference sheets or guides that
the participant may need.
The following are stylistic examples of possible prerequisites,
customize these for each workshop.

### Required Downloads and Installations

List any required downloads and installations here.
Make sure to include tutorials on how to install them.
You can either make your own tutorials or include a link to them.

To complete this tutorial, download Arduino version 1.8.16 by completing the following steps:

1. Go to https://www.arduino.cc/en/software/OldSoftwareReleases/
2. Click and Install the version for your operating system by following the built-in guide
3. Nice! You have downloaded all the software needed to complete this tutorial
4. Note: Version 1.8.16 is used because it includes an easy to use serial monitor

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
| RYLR998        |    2      |
| --------------   ---------
| RYLR993        |    2      |
| ---------------  ---------
| Male to Female |    8      |
| Jumper cables  |           |
| --------------   ---------
| CP2102 USB to  |    1      |
| TTL 5PIN Serial|           |
| Converter for  |           |
| UART STC 3.3V  |           |

### Required Tools and Equipment

List any tools and equipment you need here.
(Ex, computer, soldering station, etc.)

- Computer
- ESP 32
- USB-C to USB-A adapter (Only if your device does not have a USB-A port)

## Part 01: Connecting the CP2102 to the RYLR998 and RYLR993

### Introduction

Briefly introduce what  you are teaching in this section.

In this section you will learn how to wire the CP2102 to the RYLR993 and RYLR998 modules. 

### Objective

- List the learning objectives of this section
- Understand how to wire the CP2102 to the LoRa modules

### Background Information

Give a brief explanation of the technical skills learned/needed
in this challenge. There is no need to go into detail as a
separation document should be prepared to explain more in depth
about the technical skills

To complete this section, you just need to make sure your
jumper cables are properly connecting each module

### Components

- List the components needed in this challenge
- CP2102
- RYLR993
- RYLR998

### Instructional

Teach the contents of this section

1. Connect the GND male pin on the CP2102 to the GND female pin of the RYLR998 using a male to female jumper cable.
2. Connect the 3.3V male pin on the CP2102 to the 3.3V female pin of the RYLR998 using a male to female jumper cable.
3. Connect the Tx male pin on the CP2102 to the Rx female pin of the RYLR998 using a male to female jumper cable.
4. Connect the Rx male pin on the CP2102 to the Tx female pin of the RYLR998 using a male to female jumper cable.
5. Connect the CP2102 USB-A port to your computer

- Note: Rx goes to Tx and Tx goes to Rx is correct and necessary for proper configuration.

## Example

## Part 02: Configuring the RYLR998

### Introduction

Briefly introduce what you are teaching in this section.

Now that every module is properly connected, you can now learn
what commands are necessary to configure the RYLR998.

### Objective

- Understand the commands needed to configure the RYLR998

### Background Information

To complete this section, you will need to open your arduino IDE
and use the serial monitor to configure the RYLR998 through the 
CP2102. The commands used in this tutorial are taken directly from the documentation. You can open the documentation to verify the 
commands being used here. The beauty of using the CP2102 is that you can simply plug and play. The CP2102 does not need to be configured like the 
RYLR998. 

### Components

- No other components needed

### Instructional

1. Open the Arduino IDE
2. Go to Tools -> Port and make sure the USB-A port is connected. ON
My side the following is displayed: . Your port name may be slightly different, but the correct choice should be fairly straigthforward.
3. Go to Tools -> Serial Monitor to open the serial monitor
4. In the serial monitor, type the following command: 
AT`
   The serial monitor should respond with 
   +OK`
The "+OK" response indicates that your wiring is correct and you can proceed with configuring the RYLR998. If the serial monitor response with "+ERR**" where ** represents a number, then you most likely have wired your modules incorrectly. You must fix this before proceeding.
5. Now, type the following 
AT+IPR=?'

5. Connect the CP2102 USB-A port to your computer

- Note: Rx goes to Tx and Tx goes to Rx is correct and necessary for proper configuration.

## Example
















### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
