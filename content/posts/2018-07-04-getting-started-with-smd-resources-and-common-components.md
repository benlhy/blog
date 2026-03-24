---
title: "Getting Started with SMD, resources and \ncommon components"
date: 2018-07-04T11:15:34+00:00
slug: "getting-started-with-smd-resources-and-common-components"
draft: false
description: "What to pick? What to chose? Your standard reference to the world of SMD parts."
cover:
  image: "/images/2018/07/resistors.jpg"
  relative: false
  hiddenInList: false
tag: ["SMD", "Getting Started", "Arduino", "PCB", "Guides"]
---

### What is it?

This is a list of all common electronics components that I use.

### Why is it cool?

It is my reference when I am picking out components, and it helps when I have a standard set of components that I use.

# Power

I use Microchip's LDO selector for my power requirements: [http://www.microchip.com/ParamChartSearch/Chart.aspx?branchID=90004](http://www.microchip.com/ParamChartSearch/Chart.aspx?branchID=90004)

### MIC5209

500mA, SOT223, good for powering beefy components if heat dissipitation is a concern, has a higher range of voltages (16V) than the MIC5219.

### MIC5219

500mA, SOT-23-5, small land pattern, a good selection for most general purpose uses, smaller voltage range (12V) than the MIC5209.

### MIC5504

300mA, SOT-235, small land pattern, slightly less power. Good if you plan to put switches in the line and they are low power switches. Cheaper, smaller dropout voltage.

### MCP1700

250mA, SOT-23, cheap, small land pattern, good when you don't have a lot of room.

# MCU

Some considerations for MCU costs are that it should have a hand-solderable variant, and should be reasonably well supported. The cost is not a big issue as compared to the ease of programming and development, but a low cost is a definite boon. Here I chose to go with the SAMD series of chips as they are the ARM chip of choice for Arduino, and I find ATMEL Studio to be a relatively conveninent option, especially because of the .

I really like TI's series of TIVA chips because they are so well documented with examples, which are readily available. However, they do not make 'lesser' versions of these chips, and I don't really need a 64 pin chip when all I want to do is to blink a couple of LEDs.

### ATSAMD10D14

![small-ATSAMD10D14-VQFN-24](/images/2018/07/small-ATSAMD10D14-VQFN-24.png)

**This board is good for projects that require only a simple signals. Eg. LED animation, Neopixel controller.** 16KB memory, 4KB RAM. $1.24/unit. Cheap, plenty of memory and GPIO pins. Good for simple projects, and better than using an Arduino/ATMEGA/ATTINY. Has easy-to-solder SSUT parts. Also has the tiny QFN-24 option if you really need the space, doesn't go much smaller than that.

### ATSAMD21E17A

![medium-ATSAMD21E17-TQFP-32](/images/2018/07/medium-ATSAMD21E17-TQFP-32.png)

128KB memory, 16KB RAM. $2.30/unit. Very available, a huge amount of memory, and has the relatively easy to solder TQFP-32 part, if your board is space-challenged, it is also available in a tiny QFN-32 footprint.

### ATSAMD51G19A

![medium-ATSAMD51G19A-VQFN-48](/images/2018/07/medium-ATSAMD51G19A-VQFN-48.png)

The latest and the greatest. 512KB memory, 192KB RAM. $4.30/unit. Basically all your code that you squeezed in the SAMD21 fits onto the RAM of the SAMD51. The smallest version comes as a QFN-48 package.

# Resistors and Capacitors

I like the 0603 size because I find that those are the smallest resistors and capacitors that I can handle comfortably. If you are going to do a lot of hand soldering, this is a very important aspect. However, keep note that larger capacitor values are more common with larger sizes. Buy the cheapest ones you can find, and make sure that they are >4V, for a good safety margin. The prices don't differ that much from 6.3V to 16V, so if you can snag a higher voltage tolerance, I'd say do it.

There are two approaches to this. You can buy sample packs from Aliexpress, which will definitely net you a nice range of capacitors, or accumulate them as your projects grow, which will get you the capacitors you need. I do like the growth method because I tend to know what capacitors I need for a given project, and they are usually the same few suspects. You also get them knowing the specs like the voltage range, which a sample pack from China does not avoid. Then again, I've been saved many times when I have forgotten to buy a critical capacitor from the sample pack so both approaches do have their benefits.

For reference, a set of capacitors that I use regularly and almost exclusively are **0.1uF, 1uF, 10uF** for power decoupling, and **22pF** for crystals. For resistors I use **1k** resistors for LEDs and **10k** resistors for pullups.

# Crystals

### Generic external oscillator, Abarcon, ABS7

32.768kHz, 12.5pF, small package, paired with 22pF capacitors, good for many ATSAMD chips that use Phase-Lock Loops (PLL) to clock at 48Mhz.

# Switches

### Generic power switch, JS202011SCQN, CK

![js2011](/images/2018/07/js2011.jpg)
This is a big gull-wing SMD switch that is good for up to 0.3A. I like this particular switch because it is easy manipulate, and is good for toggling.

### Generic reset switch, TL6330AF200Q & variants

![MFG_TL6330AF200Q_sml](/images/2018/07/MFG_TL6330AF200Q_sml.jpg)
Standard reset button. Small and compact. Some variants can be a bit flat and hard to press at times. Make sure that you solder these on towards the end because the hot air tends to melt the plastic buttons.
