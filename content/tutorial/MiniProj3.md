---
title: Mini Project 3 - blinking lights to a rhythm
date: 2025-05-19
authors:
  - name: Allan Dong
---



## Introduction

This tutorial will serve as a guide on how to program a esp32 to make a led blink at a certain rhythm. 
I would like readers to learn how to program simple things on the esp32. I wanted to do this since I always liked
programmable led's. 

### Learning Objectives

-python
-esp32 pin control

### Background Information

Any audio file can be seperated into its individual notes easily when converted to .mid(Midi) format. A Esp32 board can be programed to control a variety of actuators and connections. I want to have it blink a led to the rhythm of a song
that the user can upload. This will blink the led to the beat. 

Pros
- Can have a cool visual for some music

Cons
- Songs with more complex beats can be harder to represent with just a few led's

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

With your ESP32 board and usb c connection, you will basically just need a way to connect your led to the pins on the ESP 32 board. 
This can be done either through direct connection or through something like a breadboard. The Esp32 board is actualyl quite complicated and can 
connect to other devices via bluetooth and Wifi, but we are pretty much only working locally for this task. 

### Required Downloads and Installations

circuit python
Arduino IDE(optional)
python mido library

### Required Components

List your required hardware components and the quantities here.

| Component Name | Quanitity |
| -------------- | --------- |
|     esp32 board|   1       |
|     led        |   1       |
| lightshield     |   1       |

### Required Tools and Equipment

-computer
-usb-c cable
-solder iron(only if you are directly connecting it)
-breadboard(optional)
-lightshield

## Steps: 
1. Setting up esp32 board
2. Installing software on your computer
3. Programming the esp32 board
4. Getting the software onto your esp32 board

## Part 01: setting up esp32 board
![esp32_board](esp32.jpg)

### Introduction

This part I will show you how to connect a led to the esp32 board, via lightshield in this case. You can also directly connect it to the pins or via a breadboard, but I won't be showing it here since using the lightshield makes it much easier. 

### Objective
 Learn how to connect simple components to the esp32 devboard. 

### Background Information

If you look at the esp32 devboard, you can see that the two sides are lined with pins that can output 3.3V to connected components. While 3.3V can be on the higher side for led's it should still be fine. If it is too bright you can add a small resistor between the devboard and the led to slightly lower the current. 

### Components

1 esp32 devbooard
1 lightshield

### Instructional

Assemble the components together. Connect the lightshield to the esp32 devboard via the header pins on the side.  

![esp32_board_withshield](together.jpg)



## Part 02: Writing the software


### Introduction

This part I will show you how to write the proper code to blink the led's to the rhythm of a uploaded song file

### Objective
 Learn how to convert songs to a usable format for the esp32, then blink led's to the songs beat. 

### Background Information

If you look at the esp32 devboard, you can see that the two sides are lined with pins that can output 3.3V to connected components. While 3.3V can be on the higher side for led's it should still be fine. 

### Components

1 esp32 devboard
1 lightshield
1 computer
1 usb-c cable 

### Instructional
First download your favorite song as a mp3 file. Then convert it to a midi file [here](https://samplab.com/audio-to-midi). With this mid file, put it into this python file
```C
{
  import mido
import json

midi = mido.MidiFile('your_midi_file.mid')
events = []
current_time = 0

for msg in midi:
    current_time += msg.time
    if msg.type == 'note_on' and msg.velocity > 0:
        events.append({
            'time': current_time,
            'note': msg.note
        })

with open('midi_events.py', 'w') as f:
    f.write('events = ')
    json.dump(events, f)

}
```
Run this file by entering python file.py(file is your file name) in the terminal. This should output a midi_events.py file that you will later put in your devboard's directory.


## Part 03: Running on the devboard


### Introduction

This part I will show you how to properly run everything you just wrote on your esp32

### Objective
 Learn how to make files, run, and execute software that you have written on the esp32 deboard. 

### Background Information

Python code on the esp32 runs just like on any other computer. There is file called code.py or main.py in your circuitpython directory that automtically runs when saved. You just need to upload the working code there and it should start running. 

### Components

1 esp32 devbooard
1 lightshield(or other led configuration)
1 computer
1 usb-c cable 


### Instructional
When you have set your esp32 devboard properly [according to this tutorial]([https://samplab.com/audio-to-mid](https://ece-196.github.io/docs/assignments/vu-meter/firmware/)i), you should have a directory called CircuitPython that you can see on your computer when you connect it via usb-c. Copy the midi_events.py file from the last step to this directory. Also if there isn't already a file called code.py, make one and add this code to it. 
```C
{
import time
import board
import digitalio
from midi_events import events  # <- generated file

# List of LED GPIOs in your specified order
led_pins = [board.IO39, board.IO38, board.IO37, board.IO36,
            board.IO35, board.IO48]  # Only using 6 LEDs

leds = []
for pin in led_pins:
    led = digitalio.DigitalInOut(pin)
    led.direction = digitalio.Direction.OUTPUT
    led.value = False  # Ensure all LEDs are off at startup
    leds.append(led)

# Start MIDI playback
start_time = time.monotonic()

current_event = 0
while current_event < len(events):
    now = time.monotonic() - start_time
    event_time = events[current_event]['time']
    note = events[current_event]['note']
    
    if now >= event_time:
        # Turn off all LEDs
        for led in leds:
            led.value = False

       
        led_index = note % 6  # replace 6 with however many led's you have
        leds[led_index].value = True
        
        # Print which LED is blinking
        print(f"LED {led_index} (Note {note}) is blinking")

        time.sleep(0.1)  # Flash duration
        leds[led_index].value = False  
        current_event += 1
  }
}
```
A few things you may need to change in this code. In lines 7-8, replace these conection names with whichever GPIO pins you have connected your led's to. Also on line 32 you will need to replace 6 with however many led's you are using. After making these changes, you can just save the file and it should run properly. 


## Example
Here is a short video of how it should be working once you do everything correctly and upload the right code. [Video here]([https://samplab.com/audio-to-midi](https://youtube.com/shorts/Ye-g7py5aYg?feature=share))





