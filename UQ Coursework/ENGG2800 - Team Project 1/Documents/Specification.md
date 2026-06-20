# Power Meter

Team Project specification 2026, semester 1

Generated date: 2026-05-12 18:34:06

Course: **ENGG2800**

---

Notes:

* For the purposes of this document, we consider "code" or "software" to refer to programs which runs on a PC and/or on an embedded system.
* This document uses the words "should", "must" and so on interchangeably to indicate a requirement of particular functionality. The use of "should" does not imply that functionality is optional.
* While we try our best to allow students to have the freedom to make their own design choices, it is not possible for the teaching staff to support every possible development system.
* This specification is a living document! Changes will occur and will be released throughout the semester. Some minor clarifications may appear on the discussion board only. You are expected to keep up to date with these.
* All assessment must only use PCBs which were ordered from ETSG (via [pinecone](https://pinecone2.uqcloud.net/)). Please see the course profile for details.
* There are a number of grade hurdle requirements outlined in the course profile - please make sure you read these *carefully*.
* You must follow all other requirements outlined in the [TP-STD standards](https://source.eait.uq.edu.au/gitlist/tp_std/tree/master/), otherwise your final product mark will be limited to a maximum of 50%.
* All assessment must only use parts from the approved suppliers! See TP-STD-003.
* You must individually commit to git in four weeks of the semester. See TP-STD-004 and Blackboard for further details.
* Did we mention that you should read the TP-STD documents? If you don't meet these requirements (as applicable), you will be limited to a maximum of 50% of the total marks available for the final demo.
* It is up to you to determine which parts are appropriate for your design (except where specific parts are required by this specification or the TP-STD documents). Not all parts available at ETSG will be suitable for use in your project, and some may appear to be useful while in reality not actually being useful at all. You should not expect ETSG to stock all parts needed to complete your design.
* We do not provide the final marksheet early in the semester, because we want you to read and digest the requirements in textual form (this is a learning outcome of this course). For this reason, the final marksheet will be made available to you in approximately Week 6 or 7 of the semester.
* Unless stated otherwise, all AC measurements are provided as and expected as Root Mean Square (RMS) form.

---

## Introduction and background
The goal of this project is to build a portable AC and DC power meter. The device is intended to measure AC (single phase) or DC power as well as a number of other metrics. The device must be able to display these metrics in real time and report the information to other devices. 

### Examples of commercially available power meters

- Power meters that plug into a single phase outlet: [1](https://reductionrevolution.com.au/products/plug-in-power-meter), [2](https://www.batterymate.com.au/products/power-energy-consumption-watt-meter-electricity-usage-monitor-equipment-240v), [3](https://reductionrevolution.com.au/products/power-mate-15-amp-pm15a)

- [Panel mount power meter](https://www.altronics.com.au/p/q0589-panel-mount-multi-function-digital-power-meter)

- DIN rail mounted power meters: [1](https://apac.socomec.com/en/p/countis-p0x), [2](https://www.amazon.com.au/eMylo-1-Phase-Electricity-Single-Phase-Consumption/dp/B083QGBGKK), [3](https://www.clipsal.com/products/circuit-protection/acti9/modular-single-phase-power-meter-iem2105-230v-63a-with-pulse-a9mem2105)

- [Meter box power meter](https://www.nxp.com/design/design-center/development-boards-and-designs/SINGLE-PHASE-METER)

## Device details

### Physical user interface

The device contains a 128x64 pixel LCD module (ETSG part 24-05-03) for showing information to the user. The backlight of the display must be adjustable by the user to exactly 5 levels, where the lowest level turns the backlight totally off, and the highest level turns the backlight fully on.

You are permitted to have up to 6 momentary push buttons to allow the user to control the functions of the device. You may optionally use a multi-position momentary joystick-like switch as some of these buttons (for example, Digikey part EG4561-ND). You may optionally use a single rotary encoder in addition to these buttons.

Your device must contain a toggle/sliding power switch which totally disconnects the power supply from the device; this switch must be on the PCB itself and not external (on a cable, etc). In addition to this, your device must have a red power LED which is always on when power is supplied to the device.

Your device must have an LED (that is not red) which blinks every time a new measurement is completed.

In addition to the LEDs described in this specification, you may have additional LEDs for debugging purposes but these must be disabled or removed for the final demonstration.

Your device must operate normally if there is no connection to the PC.

### DC measurement

Your device is required to measure and compute a number of DC-related measurements:

1. DC voltage (in V)
2. DC current (in A)
3. DC power (in W)

To measure DC power, all current for the device under test will be passed into **DC Current Input** and then out of **DC Current Output** (which will also be connected to GND). For voltage measurement purposes, the GND of the DC system also will be connected to **DC GND**. Note that in testing, the voltage of **DC Current Input** >= **DC Current Output**, and **DC Current Output** must always be connected to **DC Voltage GND**. 
A diagram of the connections is below (the upper diagram is how your device will be connected; the lower diagram is how your device will *not* be connected).

![DC Power Measurement](https://source.eait.uq.edu.au/gitlist/teamproject/2026s1_spec/raw/HEAD/images/DC_Power_Measurement.svg)

The largest voltage that will be applied to your device is  10V DC. The largest current that will be sent through your device is  2.2A. However, we only expect your device to be able to measure up to  9V and  2A respectively.

Although it won't be tested directly, it is very likely that during your development you will accidentally connect the current input/output backwards - you should consider how to protect your circuit from damage in this case.
Measurements must be completed at a rate of 0.5Hz or faster.

### AC measurement

Your device is required to measure and compute a number of AC-related measurements:

1. AC voltage (in V)
2. AC current (in A)
3. AC frequency (in Hz)
4. AC phase difference between voltage and current waveform (in degrees)
5. AC real power (in W)
6. AC reactive power (in VAR)
7. AC apparent power (in VA)
8. AC power factor (unitless, between -1 and 1)
9. AC peak-to-peak voltage (in V, not RMS)
10. AC peak-to-peak current (in A, not RMS)

For the purposes of calculating items 1-8 above (inclusive), you may assume that the waveform is sinusoidal. However, it is nonetheless possible that non-sinusoidal waveforms will be present on the input of your device. Although the measurements will not be correct for items 1-8 in this case, your device must not be damaged by this scenario and must continue to operate normally.

When performing AC measurements, it is possible that if the waveforms are small (or zero), it will not be possible to accurately calculate some measurements based on phase (for example, power factor). In this case, your device should show "UL" (Under Limit) to indicate that it isn't possible to calculate these quantities accurately. The specific cases where this should be shown are:

- For AC current, if the current is below 0.025A (the device should be in the low range in this case)
- For all other measurements that depend on AC current, if AC current is UL then the measurement is also UL

You may optionally also use UL for AC voltage for values under 0.25V (post-transformer). You can deviate from the UL critera listed here as long as you can justify why they would be reasonable in a product. If you are unsure about what is appropriate, please ask on the discussion board.

For safety reasons, your device will operate through a provided transformer interface box that connects to 240V AC. This box contains a [Clamp or Current Transformer (CT)](https://en.wikipedia.org/wiki/Current_clamp) that can be used to measure current, and a step-down transformer to reduce the voltage to a safe level (<20V AC). In a commercial device, this interface box would likely be integrated into the product itself. 

The secondary side of [the step-down transformer](https://www.digikey.com.au/en/products/detail/triad-magnetics/F-313X/5032119) will be connected to **AC Input 1** and **AC Input 2** on the test harness, and [the CT](https://www.digikey.com.au/en/products/detail/seeed-technology-co-ltd/101990059/5775191) will be connected to **CT Input 1** and **CT Input 2**. This means that in order to display to the user the actual voltage measurement, it will need to be multiplied by a user-configurable scaling factor (which is the turns ratio of the step-down transformer, N).

A diagram of the connections is below.

![AC Power Measurement](https://source.eait.uq.edu.au/gitlist/teamproject/2026s1_spec/raw/HEAD/images/AC_Power_Measurement.svg)

The maximum voltage range that will be applied to the AC voltage input (ie after the step-down transformer) on your device is 10V (though closer to 9V is more likely to be what the device sees in almost all circumstances). The maximum AC current that will be monitored by the current clamp is 10A +/- 1A (though due to the [burden resistor](https://tp-info.uqcloud.net/link/217#bkmrk-measuring-ac-current) integrated into the clamp, 10A will be presented to your device as 1V).

The intended AC frequency range applied to the input of the device will be between 20Hz and 100Hz. Your device does not need to report accurate measurements if the frequency is beyond this range, but it must not be damaged. 

Your device must have both a hardware low and high current measurement range to allow more accurate measurement of current. In the low current range, the Full Scale Range (FSR) of the current input must be 0 to 1A, and in the high current range it must be 0A to 10A. The device must automatically switch between these ranges depending on the input from the CT without any user intervention. Your device must have a user interface item which indicates the range that the device is in (LED, icon on LCD, etc).

Measurements must be completed at a rate of 0.5Hz or faster. 

If the input signal changes such that the current range needs to also change (ie, previous sample it was 0.5A, next sample it will be 2.5A), it is acceptable if there is a glitch/dropped measurement for a single measurement cycle as the range adjustment is performed.

### LCD module interface

The display must have a measurement screen that is able to show any four of the measurements outlined above. There must be a mechanism for the user to choose which of these items to show and in which order, and it must be possible to adjust the system to view any combination of four items using the on-device controls only. The selected items must be stored in non-volatile memory; when input power is disconnected and reconnected to the device, the measurement configuration must remain the same.

Note that any combination of items may be visible on the display - your device must be able to measure/compute them all at the same time. You may optionally allow (or prevent) the user from displaying the same quantity multiple times.

Each of these measurements should have a maximum error of +/- 2.5% (of the full input range). They must be displayed to a minimum of two decimal places or four significant digits (either option is acceptable, it is up to you to pick one). 

There must be a settings screen that allows the user to enter the transformer turns ratio (step-down ratio) for use in calculating the AC measurements. The user must be able to enter a 5 digit value (2 digits before the decimal place, and 3 digits after).


### Power measurement interface

To connect to the sensor inputs, a 5.08mm pluggable screw terminal interface will be used (socket ETSG part is 16-58-02, datasheet is [here](https://www.lcsc.com/datasheet/C5188025.pdf)) as a connection/test harness. The pinout of this harness is as follows:

1. AC CT Input 1
2. AC CT Input 2
3. AC Voltage Input 1
4. AC Voltage Input 2
5. DC Current Input
6. DC Current Output
7. DC Voltage Input
8. DC Voltage GND

During the final demonstration, you will be provided with the pluggable block with these signals already connected. Note that your device will be tested using this configuration (where voltage and current measurements are independently floating), as well as a waveform generator to simulate various scenarios. Simulated voltage will come from channel 1 and simulated current will come from channel 2 of the waveform generator. Both channels share a common ground (GND will be connected to both AC CT Input 2 and AC Voltage Input 2). Your product should work with both the test jig and waveform generator.

The pin numbering for this connector can be seen below.

![Screw Terminal Pinout](https://source.eait.uq.edu.au/gitlist/teamproject/2026s1_spec/raw/HEAD/images/screw_terminal_pinout.jpeg)

### PC communication

Your device must use the [polyglot-turtle-xiao](https://github.com/jeremyherbert/polyglot-turtle-xiao) firmware with a Seeeduino Xiao (ETSG part 24-01-01) as the only means of  communication with the PC.


No other form of communication with the PC is permitted. When the device is connected to your PC software, the LCD module must show a pictographic icon which is descriptive of this connection (for example, a USB logo). An icon should not be visible if there is no connection to the PC software. 

### Power supply

Your device must be powered by the lab bench power supply via a 3 pin Molex KK connector. Although the order and configuration of pins is up to your team to decide, it is expected that these three pins will be used for a positive supply rail, a negative supply rail and GND.


### Real time clock 

The device must have a real-time clock (RTC) to keep track of the date and time even when the device is not connected to a source of power. The time and date must always be visible on the LCD module, even if the time has not yet been set. 

 The RTC is to be powered by a CR2032 series coin cell battery.  Note that this should not power the rest of the circuit when the device is not connected to a source of power, ie it should only power the RTC. It is acceptable if the RTC is powered by other sources of energy when available, and the  CR2032  is used as a final backup.


## PC software

The PC software must be able to view all measurements that the device is performing (even those which are not visible on the LCD) in textual form and these should update live while the device remains connected. The user must also be able to select a single measurement (of the possible measurements) to be plotted live on a graph - the graph should have the measurement quantity on the Y axis, and the current time offset on the X axis (0, 1, 2, 3, etc) in second from when the device first connected and the first point was received. You must also implement functionality that allows the user to hover the mouse pointer over the plot to see the exact RTC time (not just the time offset from zero) that the sample occurred at, as well as the exact measurement in textual form at that time.

The update rate should be approximately as fast as the measurement rate on the device.

If the measurement point could not be completed due to a "UL" error, it must be clearly marked as such on the plot (using a different point icon, different colour, etc).

There must be controls to choose the time range to be plotted (in the range of 10sec-3600sec). Once the maximum time range is reached, the oldest point is discarded when plotting a new one.

All measurements must be recorded in the background so that when the user selects a different measurement, the graph can show all of the most recent points, even those points that occurred while a different measurement type was selected.

There must be a button to send the current PC time to the device; the device should store this in the RTC if received.



## Construction and physical dimensions

Your final product must be constructed using one or more custom designed PCBs. All components (display, buttons, etc) must be mounted on or to the PCB - no other mounting frames (3D printed, laser cut, etc) are permitted.

**Submitting your product using any breadboards will result in your maximum mark for the final demo being limited to 50%.** Please be aware that PCB submissions are only possible to [pinecone](https://pinecone2.uqcloud.net/) up until the end of week 10, and you cannot use PCBs from other sources as part of your submission. As such, **you must submit a final PCB design (if you intend to submit with a PCB) to pinecone by the end of week 10** - no extensions of this manufacturing date are possible because it would impact the delivery of PCBs to other teams.

The following breakout boards are approved for use without triggering the grade hurdles outlined in the course profile:

* Seeeduino Xiao (running the polyglot-turtle-xiao firmware only)
* The display module mentioned previously
* Any breakout board which contains only an amplifier (op-amp, power amp, etc) and/or decoupling capacitors

## Budget/Bill Of Materials (BOM)
Your final product must have a BOM total of $150 AUD or less (excluding GST). Your team will have a $200 AUD development budget available at ETSG; you can spend your ETSG development budget in one of three ways:

1. At the [ETSG store](https://etsgstore.uqcloud.net) (on campus in 50-S309), see also [How to use the ETSG store](https://eecs.uq.edu.au/files/16775/ETSG%20Web%20Store%20-%20HOW%20TO%20GUIDE%20with%20mobile%20app.pdf)
2. Using [pinecone](https://pinecone2.uqcloud.net) to order PCBs
3. Using [hazelnut](https://hazelnut.uqcloud.net) to order parts from [DigiKey](https://www.digikey.com.au/)

Alternatively, you can order directly from any of the approved suppliers in the TP-STD documents (or a non-approved supplier as long as an equivalent part is available at an approved supplier), but **reimbursement will not be possible**.

You do not need to include the cost of the  USB cable in your BOM, but you must still provide it with your submission.


Any screws or mounting hardware that you have chosen for your product must be included in the BOM.

## Components included in your locker
* The display module mentioned previously
* A Seeeduino Xiao to be used with the [polyglot-turtle](https://github.com/jeremyherbert/polyglot-turtle-xiao) firmware.
* Various jumper wires
* ATMEGA328P (ETSG part 10-03-03): [https://www.digikey.com.au/product-detail/en/microchip-technology/ATMEGA328P-PU/ATMEGA328P-PU-ND/1914589](https://www.digikey.com.au/product-detail/en/microchip-technology/ATMEGA328P-PU/ATMEGA328P-PU-ND/1914589)
* AVR ISP breakout (ETSG part 16-56-01): [https://www.adafruit.com/product/1465](https://www.adafruit.com/product/1465)


## Other notes and recommendations
The following are not necessarily requirements for the project, rather they are hints and tips that may make things go more smoothly for you.

- You should be powering your MCU with 3.3V (or lower).
- You should search and peruse tp-info for further information on these topics.
- The mating terminal connector we will provide can be seen [here](https://www.lcsc.com/datasheet/C5187918.pdf). The ETSG part number is 16-58-01.
- We have new AVR MCUs available with significantly more flash and RAM - AVR128DB28 (ETSG 10-14-02). However, these MCUs are not code-compatible with the MCUs you are used to from CSSE2010 (ATmega328P, etc) and use a fairly different register access syntax. You should only be using these if you feel very confident with microcontroller programming.
