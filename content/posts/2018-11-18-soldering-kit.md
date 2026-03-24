---
title: "Learn how to solder!"
date: 2018-11-18T21:40:24+00:00
slug: "soldering-kit"
draft: false
description: "I made a soldering kit for NUSTARS."
cover:
  image: "/images/2018/11/Capture.JPG"
  relative: false
  hiddenInList: false
tag: ["soldering", "kit", "555 timer", "blinking", "teach", "learning"]
---

I like soldering and occasionally I have to teach people how to solder. However, when people first learn how to solder, you don't want those joints on production systems. My solution has always been to pull out old PCBs, which come in stacks of 10 and have them learn how to solder random joints/wires together.

However, the issue is that without something to call their own, most participants aren't very interested in soldering after their first few joints, and I often end up with a pile of PCBs that I have to throw away.

There are kits that you can buy online, but I didn't like them enough, and they are usually expensive if you want to teach 10 or more people how to solder. I wanted something simple, but it had to do something interesting, so I decided that a 555 blinkie would be reasonably interesting and worth assembling on a PCB.

![](/images/2018/11/image.png)

This is a soldering kit that I made for NUSTARS, the Northwestern Rocketry Team. This kit is meant to teach someone who has not picked up a soldering iron before to solder. On the board is a 555 timer circuit that blinks an LED, with a switch and a CR2032 battery for power management. There is a mounting hole on top of the PCB for hanging it off a keychain. This circuit has been breadboarded and tested to yield blinking in the 1~3Hz range. The blinking can be slowed down by changing the capacitance or resistor values.

The curves were not easy to make in EAGLE and I decided to break them up into lines using InkScape and map those to lines in EAGLE. In this case since I wasn't to make a lot of curved shapes, I decided to take a shortcut and overlaid the image of the shape in EAGLE and traced it out. I think the result is quite good for a simple shape like this, and it didn't take me more than 20 minutes.

I think it is a nifty little PCB and I am definitely looking forward to assembling it.

Let me know if you are interested!
