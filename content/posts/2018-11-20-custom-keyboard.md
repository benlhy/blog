---
title: "Custom Keyboard"
date: 2018-11-20T06:35:37+00:00
slug: "custom-keyboard"
draft: false
description: "Tired of typing? Make a custom keyboard in python!"
cover:
  image: "/images/2018/11/customkeyboard.JPG"
  relative: false
  hiddenInList: false
tags: ["Keyboard", "Cherry Mx", "Adafruit", "Itsy Bitsy", "Python", "custom"]
---

This was a quick and simple build. I wanted to play around with some Cherry MX keys, the ones used on gaming keyboards, and I decided the best application would be to customize each key press on my tiny keyboard to register as a series of strokes. The microcontroller here used is the Itsy Bitsy from Adafruit, currently the cheapest, and the best implementation of an easily accessible microcontroller on the market. Programming is done via python, and this microcontroller emulates a keyboard, so when I depress a button on my tiny keyboard, the microcontroller actually sends a pre-set number of keystrokes (as if I was typing really quickly on an external keyboard) to the computer.

![](/images/2018/11/image-1.png)

I also implemented a rotatory encoder so that I can change the mapping of each key for each application. Here I used a Neopixel strip to indicate which map I was using, but I think an 8 segment display would have been much better. It is programmed in CircuitPython - Adafruit's implementation of micropython and I have been liking how easy it is to get started.

I think the best use case for this is a key/shortcut that you need just often enough, but not so much that it is muscle memory. Or fun things like rotating through your most recently visited websites.
