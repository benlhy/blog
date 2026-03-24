---
title: "TFT ST_7735 Screen hookup guide"
date: 2017-12-29T04:48:36+00:00
slug: "tft-st_7735-screen-hookup-guide"
draft: false
cover:
  image: "/images/2017/12/IMG_20171228_223020-1.jpg"
  relative: false
  hiddenInList: false
tag: ["Guides", "Screen", "TFT", "LCD"]
---

This is how I hooked up an ST 7735 TFT LCD module to an Arduino. This is a small, cheap screen with color display, and an excellent Adafruit library has already been written for it, making it a breeze to use.

I recently purchased a TFT screen with the intention of using it eventually. However the markings on the back confused me a little. These are the mappings from the generic red SPI TFT to the Adafruit breakout.

![IMG_20171228_222903-1](/images/2017/12/IMG_20171228_222903-1.jpg)

Aliexpress -> Adafruit -> Arduino
LED -> Lite -> Any
N/C -> MISO -> NIL
SCK -> SCLK ->13
SDA -> MOSI -> 11
AO -> D/C -> 8*
RESET -> RST -> 9*
CS -> TFT_CS -> 10*
GND -> GND ->GND
VCC -> VCC -> 5V

All numbers with * can be changed. The default number is provided.

Once done, the Arduino can be used with the Adafruit ST_7735 library for the SPI, 1.44in screen. It worked for the screen I was using. It was advertised as *[1.44 inch Serial 128x128 SPI Color TFT LCD Module Instead of Nokia 5110 LCD](https://www.aliexpress.com/item/1-44-inch-Serial-128-128-SPI-Color-TFT-LCD-Module-Instead-of-Nokia-5110-LCD/32384540120.html)* on Aliexpress.

I used the Teensy 3.6 to run it without problems. I am looking forward to using it with a larger screen and perhaps running a video file on it.
