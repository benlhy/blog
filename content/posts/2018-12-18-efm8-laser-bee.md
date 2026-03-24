---
title: "EFM8 Laser Bee from Silicon Labs"
date: 2018-12-18T18:08:20+00:00
slug: "efm8-laser-bee"
draft: false
description: "An 8-bit offering from Silicon Labs with an impressive amount of documentation and support to get you up and running fast."
cover:
  image: "/images/2018/12/IMG_20181217_220308_HDR.jpg"
  relative: false
  hiddenInList: false
tag: ["Silicon Labs", "EFM8", "8bit", "8051", "Laser Bee", "Microcontroller"]
---

# Why is this cool?

It is a low cost microcontroller from microchip manufacturer **Silicon Labs** in the US that could find many uses for small projects as an alternative to the SAMD10 or PIC16. This is my first microcontroller that uses the 8051 from Intel.

The **EFM8** family comes with multiple variations of the same chip, tailoring them to various tasks. The **Universal Bee** has USB capabilities and an integrated 3.3V LDO regulator, the **Laser Bee** is targeted towards analog and digital sensing functions, featuring a 14 bit ADC and a 4 channel 12 bit DAC, the **Sleepy Bee** is a low power specialist, and the **Busy Bee** serves as the generalist of the bunch.

# First impressions

The EFM8 Laser Bee PCB is gorgeous. The pins are surface-mounted, so there aren't any prickly bits on the bottom, making the PCB very comfortable to hold and manipulate. However, one quibble is that you will need male-female wires to prototype anything on a breadboard because of how the pins are broken out.

{{< figure src="/images/2018/12/IMG_20181217_220808_HDR.jpg" alt="" caption="EFM8 Laser Bee Development Board" >}}

It comes with a low power LCD and hardware to monitor the power that the Laser Bee is drawing. This is integrated closely with the IDE that Silicon Labs provides and you can even view a live trace of the current draw.

{{< figure src="/images/2018/12/image-8.png" alt="" caption="Simplicity Studio Energy Profiler Perspective" >}}

# Development

Development is done within the Eclipse-based **Simplicity Studio**, and Silicon Labs provides the Keil compiler for their chips completely free of charge. Simplicity Studio is cross-platform and installation is a breeze.

> Note that for Linux users, you will have to install `wine` and `libqt4-network` separately for Simplicity Studio to work correctly.

For the library/examples, there is no need to download and install the software development kit separately, everything is handled automatically within Simplicity Studio. I tried the examples in the launcher and was pleasantly surprised that they work without a hitch and actually come with really helpful comments and documentation. The examples are loaded on the launcher perspective when you select your board.

{{< figure src="/images/2018/12/image-9.png" alt="" caption="Launcher perspective" >}}

You would think that this is a given, but often enough I have come across old code or plain broken examples that looked like they were hacked together from an example for another chip. Sometimes they don't even exist at all and you are expected to work out the details yourself from a single 'general' example for the entire family of chips. Happily, this is not the case with Silicon Labs.

{{< figure src="/images/2018/12/image-10.png" alt="" caption="Helpful comments in the ADC example" >}}

There isn't much in the way of a standard library like HAL from ST Microelectronics or the ASF from Atmel, so most of the programming/setting up peripherals comes from register programming. It does require some familiarity with programming registers to get started quickly, but if you know how to toggle a pin using registers, then you should be fine. Newcomers from Arduino might be a little lost as to where to start, but both the useful documentation as well as the `Open Declaration` feature in Simplicity Studio should quickly help you get started.

# Conclusion

The EFM8 development board is a joy to work with. With useful debugging tools, a smooth usage experience, and plenty of documentation, I would highly recommend this as the next new thing to try it out. If you have some experience with Arduino this might be a good next step into embedded development. The targeted features of this chip combined with their low cost make them highly attractive as mid-level workhorses, good in small systems or working in concert with more powerful chips in more complex systems.
