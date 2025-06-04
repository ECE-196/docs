---
title: DIY Todo List
date: 2025-05-19
authors:
  - name: Elton Villalta
---

![content/tutorials/LCDTodoLogo.jpg](LCDTodoLogo.jpg)

## Introduction

- I am aiming to create a LCD todolist that will just sit at my desk
What is the motivation behind the tutorial?
- I tend to forget a lot of the things I need to do throughout the day especially if its not in front of me.
What do you want readers to gain from the tutorial?
- Power. Aside from that, i hope readers can learn how to program a LCD display and be become more confident in wiring on a bread board.
### Learning Objectives

- Bullet list of skills/concepts to be covered
- Programming
- prototyping
  
Any additional notes from the developers can be included here.

### Background Information

Describe your topic here. What does it do? Why do you use it?
Are there other similar things to use? What are the pros and cons?
Explain important concepts that are necessary to understand.
Include (and cite if needed) any visuals that will help the audience understand.
Its just going to be a todo list that is displaying tasks i need to complete for the day. There are other things similar to use but this could be made with common parts .

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



Arduino IDE
(some driver i cant remember of the top of my head)

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
|        bread board        |      1     |
|       1602A lcd         |    1       |
|          esp32s3      |       1    |
|          jumper wires       |      25     |
| type c to type c cable| 1 |


### Required Tools and Equipment

List any tools and equipment you need here.
- need a laptop

## Part 01: 

### Introduction

We will be learning how to make our own todo list using a lcd and our esp32 board! I will break it into a few parts.


### Objective
- learn how to program the lcd display with text
- assemble complete breadboard circuit

### Background Information

All you need to complete this tutorial is some basic arduino knowledge 



### Components
- Breadboard
- 16x2 LCD
- ESP32S3
- Type C to Type C cable
  
### Instructional
Go to this website to download Arudino IDE if not installed already.
https://www.arduino.cc/en/software/

Once downloaded, open it up

Go to File>New Sketch

Copy-Paste this code into the file

``` #include <LiquidCrystal.h>
// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(17, 16, 15, 14, 12, 11);
void setup() {
  // set up the LCD's number of columns and rows:
    lcd.begin(16, 2);
}
void loop() {
  // set the cursor to column 0, line 0 and print a message.
  lcd.setCursor(0, 0);
  lcd.print("Do ECE 196 HW 1");  // you can change it into any text
  // set the cursor to column 0, line 1 and print a message.
  lcd.setCursor(0, 1);
  lcd.print("");    // you can change it into any text
}
```
Then we want to make sure our esp32 is plugged in to our laptop using a type c to type c cable. If your laptop does not support a type c connection, you may use a regular usb connection.

We then want to make sure we select the correct board.

![insert pic of board selection menu]

once we select the correct board, we can now jump over to setting up the breadboard circuit.




## Example

### Introduction

So here we have the LCD fully hooked up and programmed displaying "Do ECE 196 HW 1"

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
