---
title: Bloch Sphere Quantum-Bit Model Tutorial
date: 05-16-25
authors:
  - name: Miki Korol
---

Testing Markdown Syntax
=======

Miki Korol
---
**Man I love markdown!**

* First ill do this
* then ill submit 

  
![image](https://github.com/user-attachments/assets/a0ae3235-d7bd-4b30-b160-83ebf1033004)
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

You will also learn how to develop the code to controll the pure state vector
to model the mechanics of real quantum bits.

Currently there are no programable bloch sphere models on the internet that can 
be found. The only bloch sphere models are static and use two magnets one on 
the inside of the sphere that looks like a vector and one on the outside to 
represent the pure state vector.





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

we will be using the ... coding program and to instal it go to ...

we also need CAD software. You can use onshape which is online. 
To be able to use onshape you just need to create a free account. 

### Required Components


| Component Name | Quanitity |
| -------------- | --------- |
|  nonmetal brarings    |     4-6      |
|  soft iron rod (.19 diameter X 1+in length)| 1|
|  coils/wire|10+ ft|
|  resistors(blank ohms ~1K)|10|
|  voltage source (battery/other)|1|
|  plastic bloch sphere enclosure(5.5 inch diameter)|1|

### Required Tools and Equipment

3D printer availability
Breadboard and wiring
computer
CAD software (onshape)
List any tools and equipment you need here

## Part 01: Large Solenoid Coil For Field Generation

### Introduction

I will teach you how a solenoid coil works and how to make it using special wiring. 
We will also learn how to set up the circuit and test for magnetic induction.

### Objective

We Need this solenoid coil to be able to control the magnetic field and thus the 
vector direction of the bloch sphere vector.

### Background Information



### Components



### Instructional

1. Wrap the uncut wiring around something celyndrical. For oure case we need N turns of wire

2. Cut the end of the coul attached to the spool of using wire cutters.

3. If your solenoid is loose you can wrap it on the top and bottom with electrical tape.

## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
