Bloch Sphere Quantum-Bit Model Tutorial
=======
06-06-25

Miki Korol
---

  
<!---![image](https://github.com/user-attachments/assets/a0ae3235-d7bd-4b30-b160-83ebf1033004)-->
![image](https://github.com/user-attachments/assets/09eee5af-fe3c-47dc-a60a-6af815638270)


## Introduction

In this tutorial you will learn a little about the states of a quantum bit 
and and how we can model it using a simple bloch sphere model.

This tutorial is aimed to make it easier to break into quantum mechanics 
and quantum technology by being able to visualise the quantum states and 
how they evolve into real world measurements. 

Readers should gain a baseline knowledge of quantum bits and their basic 
mechanics. They should also walk away knowing a little about magnetic field
solenoids, ferromagnets, and programming/controls.

### Learning Objectives

- Understand the basic principles of qubits and quantum information

- Learn why we use qbits and whet they are made of

- Learn about the bloch sphere representation of quantum bits

- Recap CAD/3D-print skills to realize the bloch sphere
  
- Learn to breadboard the circuit for controlling the bloch sphere

- write the code to control magnetic sphere

### Background Information

Similar to a binary bit, when they are measured, quantum bits can either take on
the value 0 or 1. However, unlike binary (AKA classical) bits quantum bits can exist 
in strange states before they are measured in which they have a certain probability 
of being in one or the other state. For example, a quantum bit can be in a state in 
which it has 30% chance of being 0 when measured and 70% chance of being 1 when measured.
The term to describe this mixed state before measuremenrt is a quantum superposition. 
When you measure the quantum bit, it is as if it samples from the superposition distribution 
and changes it's state to the sampled one.

To mathematically represent these states we generally use dirac vector notation. For a pure 
quantum bit |Ψ⟩ in the superposition state, it can be represented by a linear combination of 
orthogonal basis vectors {|0⟩,|1⟩} like so: |Ψ⟩ = a|0⟩ + b|1⟩ where a and b are the 
coeficients of the basis vector and their magnitude represents the square root of the 
probability of the quantum bit becoming that state when measured. So to represent the quantum 
bit state we described earlier we can write it as 
|Ψ1⟩ = $(\sqrt{0.3}) \ |0\rangle + (\sqrt{0.7}) \ |1\rangle$

I may also mention that for reasons that we will not go into during this tutorial
the numbers a and b are also complex numbers. a=c+di and b=x+yi. This is necesary 
to explain some more complicated quantum mechanical properties that we will not get 
into. One experiment that displays the complex naure is detailed here 

https://en.wikipedia.org/wiki/Aharonov%E2%80%93Bohm_effect.
 
The bloch sphere is a unit radius sphere on which we map quantum bit states. The vertical axis consists
of the $|1\rangle$ going down and the $|0\rangle$ going up. This may be counterintuitive because 
the orthogonal vectors lie 180 degrees apart. This, however, is because quantum states exist in a different
space than 3 dimensional space called the hilbert space. due to the complex nature of coeficients a and b
the vectors lie on this 3 dimensional unit sphere. In this tutorial we will confine our bloch sphere to 2 
real dimensions inside of the bloch sphere. 
![image](https://github.com/user-attachments/assets/d61fc568-21f8-434d-a10b-81b40e988bba)


You will learn how to implement a physical representation of a 
simplified version of bloch sphere. To do this we will use a gimble at the 
center of which will be the bloch sphere vector. To realize this we will 
use a solenoid coil and suspended soft iron bar which will represent the pure 
state vector. To control the pure state vector we will run current through the 
solenoids to apply magnetic fields and thus a force on the permeable material.

For time and complexity reasons we will first model just the up spin and down spin of the 
bloch sphere before jumping into the whole thing. This means that the compas needle will point either up or down for now starting from the "50%50%" state AKA sideways.

You will also learn how to develop the code to "measure" the pure state vector
to model the mechanics of real quantum bits.

Currently there are no programable bloch sphere models on the internet that can 
be found. The only bloch sphere models are static and use two magnets one on 
the inside of the sphere that looks like a vector and one on the outside to 
represent the pure state vector.


### Required Downloads and Installations

You need to download an IDE that has circuit python 2.0. I used VS code which you can download here:

https://code.visualstudio.com/download

To download circuitpy head to extensions and look up Circuitpy 2.0 and download the latest version.
To flash the ESP 32 and connect the circuit to the code you can follow the steps from this github page: 

https://ece-196.github.io/docs/assignments/vu-meter/firmware/

we also need CAD software. You can use onshape which is online. 
To be able to use onshape you just need to create a free account. 

### Required Components


| Component Name | Quanitity |
| -------------- | --------- |
|  nonmetal brarings    |     4-6      |
|  soft iron rod (.19 diameter X 1+in length)| 1|
|  coils/wire|10+ ft|
|  resistors(blank ohms ~1K)|10|
|  LED(RED)|5|
|  LED(GREEN)|1|
|  High voltage/current source|1|
|  10W 1 ohm ceramic resistor|1|

### Required Tools and Equipment

3D printer availability
Breadboard and wiring
computer
CAD software (onshape)

## Part 01: Large Solenoid Coil For Field Generation

### Introduction

I will teach you how a solenoid coil works and how to make it using special wiring. 
We will also learn how to set up the circuit and test for magnetic induction.

### Objective

We Need this solenoid coil to be able to control the magnetic field and thus the 
vector direction of the bloch sphere vector.

### Background Information

Solenoid coils have been used since their invention in 1823 for a veriety of applications including but not limited to solenoid actuators, transformers, degaussers, and more. They are bassed uppon the princible of Ampere's law which relates the current to the induced magnetic field. You can read about solenoids on the wilepedia page:

https://en.wikipedia.org/wiki/Solenoid#:~:text=A%20solenoid%20(%2F%CB%88so%CA%8A,current%20is%20passed%20through%20it. 

When you weap a coil the summ of all of the fields induced from the individual currents in the wire can produce a high magnitude magnetic field in the center of the coil. 

We will use that concept to apply fields to the iron rod which will roatate to align its grains with the field in the lowest energy state. 

### Components

1. Voltage Source

2. High Guage Wire (Rated To 30-60+W)

3. Coil Holder (3D printed or in air)

4. Iron Tod (Testing)

5. Electrical Tape

### Instructional

1. 3D print a coil holder or find something cylindrical that is big enough for the axel and iron rod to fit inside. (See below on what to design the coil holder like)

2. Wrap the uncut wiring around The 3d print or the celyndrical part. In my case to get enough force I used over 40 turns around the celyndrical tube.

3. Cut the end of the coil attached to the spool of using wire cutters making sure to leace some wire to attach to the circuit.

4. If your solenoid is loose you can wrap it on the top and bottom with electrical tape every few turns. 

## Example

![image](https://github.com/user-attachments/assets/f83f9b88-1b96-42a2-aff2-7ee6909ec441)

This is the coil that I used. I ended up having to add more turns of the coil to increase the field and thus the force on the iron rod. If you dont have a coil holder 3D part the coil may be extra loose so you may hevr to use more electrical tape.






## Additional Resources

To design the coil holder use onshape and extrude a celyndrical shell about 1 or 1/2 inch making sure to hollow the middle. You can rib the outside of the cylindar to make a slot for the wires to not slip out while wrapping. 

<img width="801" alt="image" src="https://github.com/user-attachments/assets/ee1d0c48-9a2d-4fd2-bf55-63ebaec9ea1d" />

I used 2 parts but this is not necesary. you will also need to print 2 berring holders and 2 axel for the iron rod. Make the part of the axel that enters the berring slightly smaller than the bearing radius so because prints tend to be slightly larger than you think (.2 mm radius extra). And the hole for the iron rod should be slightly larger than the rod. Same with the part you place the bearing in. Make it slighty larger than the outer bearing radius. This is what I came up with which works.

<img width="649" alt="image" src="https://github.com/user-attachments/assets/627a046b-bcc9-4928-a8b4-78bbbddc2837" />

for Onshape tutorials visit the Onshape learning center (or consult ming)

https://learn.onshape.com/  

### Assembly

To assemble the setup place the bearings into the bearing holders, insert the iron rod into the axel, and insert the axel onto into the bearings like so:  

<img width="338" alt="Screenshot 2025-06-06 at 2 06 58 PM" src="https://github.com/user-attachments/assets/70cd966e-4dce-4668-b3f8-68894c80fbc8" />



place the coil around the bearing holders on a raised surface so that the iron rod can rotate freely. Connect one end of the coil to the positive terminal and the other to the 1 ohm 10W resistor and then back to the negative terminal of your power source like so:



<img width="366" alt="Screenshot 2025-06-06 at 2 58 33 PM" src="https://github.com/user-attachments/assets/9cfabf3e-e4b8-4a80-b655-f8f7e797421d" />
(Make sure ALL of your cables and resistors are rated for high power)

Find a current level that is sufficient to move the state vector (iron rod) from the 50/50 state to the up (1) or down (0) state:

![image](https://github.com/user-attachments/assets/c7f3e013-98b3-4720-aa40-3b1133c8c746)

Up (1) State

![image](https://github.com/user-attachments/assets/ba7b591f-e642-4d9d-8213-c361c84d76dc)

Down (0) State
### Q-Bit State Circuit

Each LED needs power from an IO pin, a ~1k Resistor, and ground rail. Find 6 free IO pins on your ESP32 and connect them likes so remembering the IO pin numbers and which one is the GREEN LED. It should look like so(red wire to ground pin and breadboard rail):

![image](https://github.com/user-attachments/assets/7dd49dbc-2b79-4cf9-a046-e0981baa3f10)


### Q-Bit State Display Code
Inside of the circuit python environment you can change the code to:

~~~
import board  
import digitalio
import random

stateLed = digitalio.DigitalInOut(board.IO21) //define Green LED for state value
stateLed.direction = digitalio.Direction.OUTPUT

ILed1 = digitalio.DigitalInOut(board.IO33)   //define RED LED's for current value display
ILed1.direction = digitalio.Direction.OUTPUT

ILed2 = digitalio.DigitalInOut(board.IO34)
ILed2.direction = digitalio.Direction.OUTPUT

ILed3 = digitalio.DigitalInOut(board.IO35)
ILed3.direction = digitalio.Direction.OUTPUT

ILed4 = digitalio.DigitalInOut(board.IO36)
ILed4.direction = digitalio.Direction.OUTPUT

ILed5 = digitalio.DigitalInOut(board.IO37)
ILed5.direction = digitalio.Direction.OUTPUT

print("Type stateGen to measure the Q-bit state:")

while True:
    try:
        cmd = input().strip()  // wait for the input stateGen from Serial monitor
    except EOFError:
        continue 
        
    if cmd == "stateGen":  //if stateGen entered run the following:
        state = random.randint(0, 1)  //measure the state efectively 50/50 because the state vector is perfectly in between 0 and 1
        stateLed.value = bool(state) //display the state on the GREEN LED 1 = on 0 = off
        ILed1.value = 1  //all 5 RED LED's on because I needed 5A of current iff less remove
        ILed2.value = 1
        ILed3.value = 1
        ILed4.value = 1
        ILed5.value = 1

        print(f"Measured state is: {state}") //print message detailing which random state was measured. then apply the current to enact the change.
        if state:
            print("apply RED LED level current in A")
        else:
            print("apply RED LED level current in A")
    else:
        print("stateGen incorrectly spelled")
~~~
Run this code and type stateGen in the terminal to generate the state like so:

![image](https://github.com/user-attachments/assets/1852d6d6-5a97-4ce0-9bcd-8e8fd4eab643)


![image](https://github.com/user-attachments/assets/5ade1b64-e431-4675-a499-24dc061f2cb2)

GREEN LED is off 5 RED LED's on so apply -5A

![image](https://github.com/user-attachments/assets/7dd49dbc-2b79-4cf9-a046-e0981baa3f10)

GREEN LED is on 5 RED LED's on so apply 5A

### Analysis

Now you have a Q-bit in which you can model 50/50 initial state and the outcome of measuring for a 0 or a 1 state. As a recap first we developed a solenoid holder and the solenoid that can apply the field to rotate the state vector. Then we developed and installed the state vector using holders, bearings, axels, and the iron rod. We then connected the solenoid to the power circuit. Finally we built the state vector display/measurement circuit and ran the code that generates the states.

To improve or continue with this project try developing a bloch sphere that can controll a 2D or 3D vector to enhance the model. this will require more coils, bearings, time, precision and more and will surely give you a good challenge. 

### Useful links

VS code installer: https://code.visualstudio.com/download

Circuit Python Instalation and Flashing Instructions: https://ece-196.github.io/docs/assignments/vu-meter/firmware/

Onshape Tutorials: https://learn.onshape.com/  

Imaginary Quantum Bit States Info: https://en.wikipedia.org/wiki/Aharonov%E2%80%93Bohm_effect.

Solenoids Info: https://en.wikipedia.org/wiki/Solenoid#:~:text=A%20solenoid%20(%2F%CB%88so%CA%8A,current%20is%20passed%20through%20it. 



