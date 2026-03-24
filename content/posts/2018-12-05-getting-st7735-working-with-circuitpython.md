---
title: "Getting ST7735 working with CircuitPython"
date: 2018-12-05T00:41:57+00:00
slug: "getting-st7735-working-with-circuitpython"
draft: false
description: "How to get the ST7735 screen to work with CircuitPython"
cover:
  image: "/images/2018/12/ST7735.JPG"
  relative: false
  hiddenInList: false
tag: ["CircuitPython", "ST7735", "Micropython", "Adafruit", "Itsy Bitsy"]
---

tldr: Load the font5x8.bin in the root rather than in the lib directory. Change _MADCTL in the ST7735.py folder to 0xc8 to flip BLUE and RED.

## How to draw text

Follow the Adafruit guide here:

[https://learn.adafruit.com/micropython-displays-drawing-text](https://learn.adafruit.com/micropython-displays-drawing-text)

Some notes on the implementation in Python:

- Load the font5x8.bin in the root folder rather than in the lib directory.
- Change _MADCTL register in the ST7735.py folder to 0xc8 to flip BLUE and RED if you have a green tabbed LCD

![](/images/2018/12/image.png)

## Thoughts

Circuit python updates the text very slowly. It might be worth it to implement a screen driver chip just to speed it up a little.
