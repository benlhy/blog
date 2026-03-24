---
title: "Don't power an Arduino Mini Clone with 12V"
date: 2017-12-26T06:45:16+00:00
slug: "blew-up-a-cheap-arduino-pro-mini-clone"
draft: false
cover:
  image: "/images/2017/12/blownarduino-2.JPG"
  relative: false
  hiddenInList: false
tags: ["smoke", "Arduino", "clone", "mini", "pro"]
---

Tl;dr, don't power Arduino Pro Mini Clones with 12V. You will see smoke.

I was attempting to rebuild my Sunrise alarm clock with an Arduino Pro Mini. However when I plugged in my 12V supply, a capacitor exploded on the board. The brown bit that you see near the bottom of the board was the component that sparked, and then started to smoke. It smelt like solder. I pulled out the power supply, but I think the board is ruined.

![blownarduino-3](/images/2017/12/blownarduino-3.JPG)

It turns out that the capacitors that chinese manufacturers use are cheap ones and for a 12V supply, the input voltage can sometimes vary. The polarised capacitors that were on the board are probably rated to 12V, so when the power supply exceeded this limit, it exploded.

I guess that goes to show cheap components can sometimes mess up your day.

# Solution

Power it from a proper regulated 5V source, i.e LM7805.

I was not the only one who faced this problem [[1]](#fn1).

- [https://arduino.stackexchange.com/questions/750/arduino-pro-mini-3-3v-version-input-voltage-range-tolerance](https://arduino.stackexchange.com/questions/750/arduino-pro-mini-3-3v-version-input-voltage-range-tolerance) [↩︎](#fnref1)
