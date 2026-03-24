---
title: "BreatheDot 2"
date: 2020-11-07T06:45:27+00:00
slug: "breathedot-2"
draft: false
description: "Upgraded BreatheDot with new power saving features and interface options for years of use!"
cover:
  image: "/images/2020/11/front.jpg"
  relative: false
  hiddenInList: false
tag: ["BreatheDot", "2020", "STM32", "meditation", "breathing"]
---

[BreatheDot by West Side Electronics on TindieA Portable Meditation Aid![](https://d2ss6ovg47m0r5.cloudfront.net/ico/apples-touch-icon-precomposed.png)Tindie![](https://cdn.tindiemedia.com/images/resize/e5dj-un2h8WaC8Vz0nIN5WY3OtY&#x3D;/p/1200x630/smart/i/14595/products/2017-09-02T14%3A31%3A28.557Z-image.jpg?1606306133)](https://www.tindie.com/products/ulcek/breathedot-2/)Extra units available for purchase
A BreatheDot for modern times. A fully featured meditation aid now comes with GPS 🛰️, WiFi 🌐, Bluetooth, and a mindfulness app 🧘.

*Just joking*! We certainly learnt our lesson from the IoT Table Lamp debacle didn't we? BreatheDot 2 features quality of life improvements (for me), and some design changes that will make it easier to use.
{{< bookmark
  url="https://westsideelectronics.com/the-most-useless-iot-table-lamp-in-existence/"
  title="The most useless Io"
  description="T table lamp in existenceHow I made the most useless IoT table lamp in existence. Now comes with mechanical and electrical design comments!"
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
It serves the same purpose as the original BreatheDot: an aid for guided breathing exercise in a small form factor. It also helps as a focus object when experiencing high stress or anxiety. The design is deliberately simple to remove all distractions and possibilities of failure.

---

# New features

## Deep sleep

Instead of having a switch, the BreatheDot now uses a SMD button to wake up the IC from a deep sleep mode. This makes the BreatheDot easier to use because you don't need to fiddle around with a tiny switch, and the entire surface of the PCB can be used to trigger a button press.

{{< figure src="/images/2020/11/back.jpg" alt="" caption="Back of PCB with button and coin cell holder" >}}

It completes three full breathing repetitions before the IC goes to sleep. I used the standby mode on the STM32, which draws a tiny amount of current when it is not actively guiding the breathing process.

It still uses a CR2032 battery, and given the current draw, it will last for about 20,000 cycles on a single battery. That means years of usage, even if you use it daily.

## IC change

Internally, it has been changed to a STM32G0, which greatly reduces the effort of programming the ATTiny85. Now the IC can be flashed directly, instead of flashing the ATTiny85 with an Arduino bootloader and then with code written in Arduino.

## Board changes

The holes that were meant to be soldered to in the original design was removed because they were too difficult to solder to. They are replaced by a set of evenly spaced test points that are more useful for a set of pogo pins to be temporarily connected with.

Unfortunately I found it a little challenging in practice to align the pins to the pads, so I resorted to soldering temporary wires to program the IC.

# References

- [STM32G0316-DISCO page](https://www.st.com/en/evaluation-tools/stm32g0316-disco.html)
- [STM32G031 page](https://www.st.com/content/st_com/en/products/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus/stm32-mainstream-mcus/stm32g0-series/stm32g0x1/stm32g031j4.html)
