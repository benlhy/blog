---
title: "Arduino LED strip shield"
date: 2018-03-29T14:44:35+00:00
slug: "arduino-led-strip-shield"
draft: false
description: "An Arduino shield for driving LED strips!"
cover:
  image: "/images/2018/03/arduinoshield1.jpg"
  relative: false
  hiddenInList: false
tags: ["LED", "LED strip", "Arduino", "Shield", "Getting Started", "wake-up", "alarm", "RTC", "nRF24l01"]
---

Ah the amazing LED strip. I've used it to build so many fun projects, from the [Sunrise Morning Alarm](https://westsideelectronics.com/sunrise-alarm-clock/). This can probably be seen as the next level of development in my projects. I wanted something that could be used easily to connect to LED strips to power and animate them, and a couple of other components that I usually would like to have in my LED projects, such as a radio and a real-time clock (RTC).

![arduinoshield2](/images/2018/03/arduinoshield2.jpg)

On this shield, LEDs are provided on all the output lines so that visual feedback can be seen immediately for troubleshooting. One thing that I forgot for this shield was some sort of capacitor for the DC jack to ensure that the power line is not so noisy, however, I have never experienced any problems when I was simply wiring up the MOSFETS directly without any sort of capacitor on the VIN line, so for the moment, it should work with short lengths of LED strips. The traces are rated for up to 3A of current, although the MOSFETS themselves are rated for 25A/35V. Talk about overkill! But they were not very expensive, so having a high factor of safety is a nice benefit for a little extra cost. I was not sure what were the limits of current on the Arduino board is, and this will vary among boards, so I provided my own just to be sure.

The NRF24L01+ has a nice SMD version that sits flat on the board, and it can talk to other NRF24 boards by using the Radiohead library. I like this chip because it is cheap and quite reliable. It is powered by it's own 3.3V regulator so there won't be any issues of brownouts.

I also included an RTC because I frequently like to turn on the LED strips at certain times. It uses the standard DS1307Z, which I extracted off a cheap chinese module that I bought. It also has a battery which allows it to keep time when the board is not powered. Initially I extracted the EEPROM on the TinyRTC module and soldered it onto my board, and wondered for the longest time why it didn't work until I checked the markings on the chip.

![arduinoshield1-1](/images/2018/03/arduinoshield1-1.jpg)

I initially faced the problem of the programmer being out of sync in Arduino when trying to program the board. After checking each pin and connection, I determined that the RESET line on my board happened to be connected to ground. However, this appears to be an isolated case because I checked that the RESET wasn't connected to ground on my other boards, so something might have gone wrong with my soldering or it just happened to be a manufacturing defect.

That's about it! The board is not very complicated, but this simplifies a lot of the wiring and combines it into one very neat package, so I am very pleased on how well it turned out.

One potential problem is that a 5V DC supply will not be able to power the Arduino directly because it requires 7-12V on the VIN pin. One solution might be to jump the VIN pin to the 5V pin to power the Arduino, but that runs the risk of burning out the Arduino. I've not fully decided on how to handle the 5V issue yet, but currently, I would just use a step-down regulator on the PWR line and feed that back into the Neopixel strip.

Full details on [Github](https://github.com/benlhy/arduino-strip-shield/tree/master).
