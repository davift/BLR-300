# BLR-300

Key Card Addon: Binary LED Reader - 300 (BLR-300)

This writeup walks through the process of solving the hardware challenge in the NSEC 2025 CTF.

**NOTE:** This is a beginner-friendly challenge that mostly requires investigating traces and understanding the direction of current flow.

## Overview

The objectives of this challenge are:

- First, fix the existing issues on the provided board so it will properly blink the LEDs.
- Second, decipher the message output by the LEDs to obtain the flag.

**Note:** The board is attached to the badge, which is then connected to a dock available next to the soldering village to output the ciphered code. The badge by itself does not light up anything. The secret message resides on the dock. I saw one participant sniffing the pins on the badge to capture the ciphered message without fixing the board. While extremely clever, the board will reveal necessary facts about some of the pins.

Overview

![Overview of the Board](https://github.com/davift/BLR-300/blob/main/PCB_Overview.jpg)

I decided to write this guide because, after talking with participants of the competition, most were not familiar enough with the challenge to give it a try.

## Beforehand

A PDF was provided: [BLR-300-Reference-manual.pdf](https://github.com/davift/BLR-300/blob/main/BLR-300-Reference-manual.pdf), which is a guide/datasheet and also provides clues about what to look for.

Read it carefully before making any modifications to the board. Make notes of the tips and clues in that document to explore.

## Pinout and Schematics

Start by drawing a schematic of the circuit. KiCad (a free and open-source application) might help, but just pen and paper can get the job done.

**NOTE:** There is a checkered pattern printed on the board to make following the traces and reverse engineering more difficult.

Datasheet Table

![Pinout Table](https://github.com/davift/BLR-300/blob/main/BLR-300-Reference-manual_Pinout.png)

Pinout (see high resolution here: [PCB Front](https://github.com/davift/BLR-300/blob/main/PCB_Front_Big.jpg) and [PCB Back](https://github.com/davift/BLR-300/blob/main/PCB_Back_Big.jpg)).

![Board Pinout](https://github.com/davift/BLR-300/blob/main/PCB_Pinout.jpg)

Schematics (see high resolution here: [PCB Schematics](https://github.com/davift/BLR-300/blob/main/PCB_Schematics_Big.png)).

![Schematics](https://github.com/davift/BLR-300/blob/main/PCB_Schematics.jpg)

Basic steps:

- Visually inspect the board and all its features/components.
  - Identify the 8 LEDs.
  - Identify the resistors connected to each LED.
  - Identify any other components and what they relate to.
  - Use a flashlight to shine through the board. This allows you to see behind the printed obfuscation pattern.

See through paint with backlight (see high resolution [here](https://github.com/davift/BLR-300/blob/main/PCB_Backlight_Big.jpg)).

![PCB Backlight](https://github.com/davift/BLR-300/blob/main/PCB_Backlight.jpg)

- Use a multimeter to:
  - Test continuity between all 12 pins of the board (some might be shorted).
  - Follow each trace on the board to test for connectivity (some might be broken).
  - Test LEDs and other diodes for polarity (some might be inverted).
  - Test the resistors (some might not be appropriately sized).
  - Cross-reference the pinout table from the datasheet with the LEDs (some might be swapped).

By performing the inspections above, most, if not all, issues can be identified.

## Issues Found

### LED 1

Testing the LED orientation with a multimeter will reveal it is incorrect.

It is necessary to desolder and solder it back. This process frequently destroys the LED because of the heat. If necessary, ask for another RED SMD LED from one of the organizers.

![Issues on LED 1](https://github.com/davift/BLR-300/blob/main/Issue_LED_1.jpg)

### LED 2

There is no connectivity between the header pin and the LED. A visual inspection may not reveal this issue because of the printed obfuscation pattern. Use the multimeter to check continuity.

It is easy to fix with an external wire to make the route.

![Issues on LED 2](https://github.com/davift/BLR-300/blob/main/Issue_LED_2.jpg)

### LED 3

Visually, it is not possible to see the polarity of the diode in series with this LED. Use the multimeter to check continuity through the components.

The highlighted diode is not necessary and can be bypassed or have its orientation inverted.

![Issues on LED 3](https://github.com/davift/BLR-300/blob/main/Issue_LED_3.jpg)

### LED 4

This LED appears to malfunction, but the circuit is shorted in multiple locations.

On the back, there is an odd trace to a hole right next to ground on the other side. Cut it with a utility knife or any sharp metal tool.

![Issues on LED 4](https://github.com/davift/BLR-300/blob/main/Issue_LED_4_1.jpg)

On the front, there are two other shorts right at the header pin. If the header is already soldered, it might be a bit trickier to cut them.

![Issues on LED 4](https://github.com/davift/BLR-300/blob/main/Issue_LED_4_2.jpg)

### LED 5

This was the trickiest issue in this challenge because it takes time to analyze what this extra circuit is doing: a PNP transistor with two 10K ohm resistors.

There is no issue with the transistor, after all. But the circuit that connects pin 3V3 to the LED does not work and needs to be re-routed.

I did not find any purpose for resistors 9 and 10, so I shorted 9 and removed 10. Maybe it was not necessary.

![Issues on LED 5](https://github.com/davift/BLR-300/blob/main/Issue_LED_5_1.jpg)

### LED 6

The manual has the note: "The LED has a tendency to die easily if lighted up for a long time." This comment implies that the resistor that protects the LED is too low or nonexistent.

A visual inspection might reveal that, while all the other LEDs have a 75-ohm resistor, this LED has a 1-ohm resistor. Just ask the challenge organizers for a replacement resistor.

![Issues on LED 6](https://github.com/davift/BLR-300/blob/main/Issue_LED_6.jpg)

### LED 7 and LED 8

These two work out of the box, which makes it easy to overlook that they are swapped. This issue will cause the encoded/ciphered message to be incorrect.

Open the bridges on the back and reroute the circuits, swapping the pins using any piece of wire.

![Issues on LED 7 and 8](https://github.com/davift/BLR-300/blob/main/Issue_LED_7_8.jpg)

## Issues Fixed

This is how the changes look after all fixes have been applied.

![PCB Final Front](https://github.com/davift/BLR-300/blob/main/PCB_Final_Front.png)

![PCB Final Back](https://github.com/davift/BLR-300/blob/main/PCB_Final_Back.png)

At this point, the PCB can be connected to the badge and attached to the dock provided by the organizers of the challenge to capture the ciphered code.

## Encoded system

There is an old Windows font that contains the same symbols printed on the back of the board. You will need to decode the message using this font to obtain the flag.

(to be continued)
