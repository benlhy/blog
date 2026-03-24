---
title: "To Buy or to Build"
date: 2021-11-28T05:23:52+00:00
slug: "on-making-vs-buying"
draft: false
description: "Some thoughts on buying vs building and the rules of thumb that I use to get by. "
cover:
  image: "/images/2021/08/original_e9bfbd1f-9fde-4a4d-8470-f6bf6ff6618d_PXL_20210827_213531275.jpg"
  relative: false
  hiddenInList: false
tags: ["2021", "making", "vs", "buying"]
---

To buy or to build is all about the tradeoffs in light of unknowns: if you buy, you can get the product you need faster, but you sacrifice flexibility later on.  You also have to consider ecosystem lock-in and being forced to build hacky workarounds to get the functionality you want. If the project turns out to be massively successful and you need a custom application, tearing down everything and rebuilding from scratch can be very painful.

On the other hand, if you chose to invest in the building, it will require a more significant upfront cost as well as time investment. This lends you the flexibility to build a solution that is customized to your exact needs, but you give up the opportunity cost of doing something else that may contribute more to your end goals. If it turns out that it is not what you needed in the first place, then all you've achieved is spending a lot of money and time.

But no one can predict the future, so it is really up to your own best judgement. As a rule of thumb, buy first, then build. The costs of building from scratch compared to buying are so high, and the chance that a generalized solution doesn't exist for your problem is so low that it almost never makes sense to build something from the outset.

- Have I searched for a similar solution and tried it for a bit?
- Are there any significant benefits or drawbacks to this solution in the context of my problem?
- What is the cost of implementing it myself?

Despite appearances, I don't build everything myself. I've been known to use the odd 3D model online rather than design the model myself.

# Background

I recently created low power module to take care of the charging and powering of remote devices. It is capable of taking in a voltage range of 0.8V to 5.5V which is commonly available from many devices and creating 3.3V to run a remote wireless device.
{{< bookmark
  url="https://westsideelectronics.com/sun-power/"
  title="Harnessing solar for sensors 🌞Proving power to wireless devices from the sun. INFINITE ENERGY!"
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
I really like it because it works quite reliably. I can also use it as a general power step-up/down power supply that will allow my circuits to operate with any kind of battery or wall power. The main IC is also relatively cheap as well - it costs around $1.20 and has an easily solderable package.

{{< figure src="/images/2021/08/image.png" alt="" caption="Breakout board" >}}

The great thing about this IC is that the output voltage is also configurable by changing a pair of resistors (R2 and R3). It is currently being used in my next project, the Simple Wireless Measurement Device.

# Buying vs Building in context

I often have the choice between making and buying, especially during the prototyping phase, because often I have a choice between making a breakout board yourself, or buying an evaluation board from the manufacturer.

{{< figure src="/images/2021/08/image-1.png" alt="" caption="MCP16415 from Microchip" >}}

Cost-wise, it is can be quite competitive to buy dev boards from the manufacturer, because they are usually offered at a discount. Add up the cost of getting the board made, buying the components, and assembling it, it usually doesn't make sense to build it myself, especially if I only need one to start with.

But this does not always hold true: the dev board for the MCP16415, the main chip I use for my solar projects, is $40 from the Microchip, and it costs me $10 to make it myself.

Building a breakout board instead of buying one does have other advantages. I can get a head start on building the circuit in hardware, which may expose problems that I might not have known about before and that I can take into first version of the complete circuit. Most boards I make take about two to three revisions before I consider them feature complete.

I can start sourcing early, and this avoids the situation where I test the prototype and I'm happy with the product, only to find out in a later stage that a critical part cannot be sourced. This happened with the Simple Wireless Measurement device where I started with the EFM12 TQFP package, but it went out of stock right when I finished the design. The fix luckily wasn't that hard - all I had to do was to include the QFN package within the TQFP package.

{{< figure src="/images/2021/11/image-6.png" alt="" caption="One package within another" >}}

On the other hand, dev boards can come packed with other features such as on board debuggers, screens, or selector switches. If the device is complicated with many options or circuitry, then I like to buy the dev board because it is faster to get started.

{{< figure src="/images/2021/08/image-2.png" alt="" caption="DRV8323RX" >}}

The DRV8323 is an example of a dev board that I would prefer to buy first to test before prototyping. Look at how complex it is! It is simply not worth my time to prototype because the complexity of the circuit and and the many features, like the MOSFETS, current measurement, and potentiometer would have taken me a long time to design. When it is in the prototyping phase where things move quite quickly, you don't want to spend days building something that you may not use.

{{< figure src="/images/2021/09/image.png" alt="" caption="EFM8 Laser Bee" >}}

The EFM8 Laser Bee is another board that takes the concept of  to the extreme. The EFM8 Laser Bee only requires 4 capacitors and 1 resistor to be used (2 capacitors if you want to be gung-ho about it). That puts your total bill at $5, if you need to buy a breakout board from Silabs, that sets you back $30. What is going on?

This breakout board packs in a J-Link debugger, a low power 128x128 bit screen from Sony, and current sense meter that ranges from 10nA to 100mA. For $30, this is an extremely good deal!

In conclusion, there are various considerations when it comes down to buying vs building. But as a whole, I usually prefer to build the board myself because it provides a lot of learning opportunities.
