---
title: "Trimpot and Switches (6)"
date: 2016-11-18T10:28:00+00:00
slug: "trimpot-and-potentiometers-2"
draft: false
cover:
  image: "/images/2016/11/trimpot-1.jpg"
  relative: false
  hiddenInList: false
tags: ["ULCEK Tutorials", "LED", "trimpot", "Guides"]
---

A trimpot is basically a resistor that you can vary the resistance of. This is useful for controlling output in an analog manner. The push switches only turn on for the length of time they are pressed.

The switches are connected across adjacent legs, which is laid out in the wiring diagram below.

![Circuit schematic](/images/2016/11/button.jpg)
Figure 1. Wiring schematic for Button
A switch is useful for sending a single `HIGH` or `LOW` pulse to a pin. To ensure that the pin is always at `HIGH` or `LOW` (so that our pulse can be detected), we want to use a pull down or pull up resistor such that when the switch is released, the pin is at `HIGH` or `LOW`.

> Warning: Do not link 5V and GND with just a switch. When you press the switch it will short circuit and this might damage your circuit.

Connect the resistor and the switch in series with a lead in the middle to the desired pin. If the resistor is connected to the 5V output, it is called a pull-up resistor. If the opposite configuration, it is then a pull-down resistor. This is because the resistor 'pulls' the signal to the pin to `GND` or `5V` when the switch is not pressed.

The wiring configuration is indicated below.

For the trimpot, it looks like the following:

![Circuit schematic](/images/2016/11/trimpot.jpg)
Figure 2. Wiring schematic for Trimpot
The outside leads are connected to GND and 5V. The middle lead is the output. How the trimpot works is that the knob is a wiper across a carbon flim that is laid out within the trimpot. As it swipes, due to potential dividing, the resistance from 5V to the lead increases/decreases. This leads to an increase or decrease in the output voltage of the trimpot.

You can use this to control the brightness of an LED manually, as seen in the circuit below.
