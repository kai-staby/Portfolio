# ENGG2800 - Team Project 1

## Overview

The high-level goal of ENGG2800, as presented on the course profile, is as follows: 

*"ENGG2800 (Team Project 1) is a learn-by-doing course that endeavours to teach issues in project management, teamwork, communication and design by giving teams of three or four students the task of designing and building a complete product encompassing both electronic and software design in a structured team environment."*

ENGG2800 is the first of two major discipline-specific team-based project courses undertaken by electrical engineering students at UQ (with the second being ENGG3800). It's a single design project run over the course of a semester in groups of four, where teams are presented with a design specification for some full-stack (hardware, software, and firmware) electronic device that must be created from scratch. A set of rigorous design standards are also provided which must be strictly followed to the letter to achieve a desirable grade. The learn-by-doing nature of the course forces teams to quickly adopt effective project management strategies, while encouraging students to dive straight into learning new technical skills across the gamut of electronic design in  pursuit of producing an interesting and practical product 

## The Project

The design brief for sem 1 2026 was to create a four-channel power-meter device which directly measured DC and AC quantities and presented useful measurements to the user. The core of the device was to be built around an ATmega328P microcontroller, with all other hardware decisions made at the group's discretion. The device was to measure and present the following within a 2.5% tolerance: 

+ DC Quantities
  + DC Voltage
  + DC Current
  + DC Power
+ AC Quantities
  + AC Voltage
  + AC Current
  + Frequency
  + Phase
  + Power Factor
  + Real Power
  + Apparent Power
  + Reactive Power

All measurements were to be taken independently and simultaneously, presented to the user via an LCD screen on the device or via serial to a software GUI on a computer. All measurements were to be logged to and graphed over time on the GUI software, with any four measurements (as decided by the user via hardware buttons) able to be viewed on the device LCD. The GUI was created in Python with tkinter, and embedded firmware was developed in C and the ATMEL AVR instruction set. 

### Contribution
I took exclusive responsibility for about half the project deliverables. Specifically, I soley designed all the measurement circuitry from schematic to layout and assembled the final product. I also worked closely with my colleague responsible for the device firmware in creating a system architecture, and embedded systems integration. Altium schematic and PCB layout artifacts can be seen at [Documents](/Documents), and the source files at [Archive](/Archive) 

## Outcome

