---
title: "Building a SAMD51 Breakout board"
date: 2018-07-23T02:14:02+00:00
slug: "building-a-samd51-breakout-board"
draft: false
description: "I made a breakout board for the ATSAMD51 - a big brother to the ATSAMD21!"
cover:
  image: "/images/2018/07/samd51_2.jpg"
  relative: false
  hiddenInList: false
tag: ["SAMD51", "Breakout"]
---

The SAMD51 is a next step up from the SAMD21, a popular series of chips that have been used for various boards. It has more memory, more space, and is a more advanced chip.

![samd51closeup-1](/images/2018/07/samd51closeup-1.jpg)

## What does it do?

It is a breakout board for the SAMD51. It takes all the pins and locates them on conveninent headers for easier prototyping.

## Why is it cool?

Microchip has not released a Xplained Board and so far the only two breakout boards are from Adafruit, which does not break out all the pins, and Mattairtech, which breaks out more pins, but not all. This breakout board breaks out all the pins while keeping the number of components as low as possible.

# Introduction

![samd51breakout](/images/2018/06/samd51breakout.JPG)

The QFN package was used for this version because it was the smallest package in terms of the number of pins. This revision adds a 3.3V regulator so that the board can be powered, and a footprint for a 32.768kHz crystal oscillator from which the board can use to drive an internal Digital Frequency Locked Loop (DFLL) which can go up to 48MHz. A USB breakout is provided to supply power, as well as an interface if desired.

Some problems with this revision are the lack of LEDs as feedback, as well as the lack of any user buttons. This will be fixed in the next revision.

![samd51](/images/2018/07/samd51.jpg)
