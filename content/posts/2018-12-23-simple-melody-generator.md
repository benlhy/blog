---
title: "Simple Melody Generator"
date: 2018-12-23T01:45:38+00:00
slug: "simple-melody-generator"
draft: false
description: "Reviving and analyzing a small simple circuit."
cover:
  image: "/images/2018/12/IMG_20181222_185055_HDR.jpg"
  relative: false
  hiddenInList: false
tags: ["simple boards", "learning soldering", "cheap ic", "salvage"]
---

# Why is this cool?

This is a melody generator that is housed in a tiny 3 pin package. It is mostly used for toys and sounds.

# Old part day

I found this music generator in the trash with some other electronics. I was initially inclined to throw it away, but I saw that it had a speaker attached to it, and a intriguing pair of connectors that said *3V* and *GND*.

{{< figure src="/images/2018/12/IMG_20181222_185055_HDR-1.jpg" alt="" caption="Board and speaker" >}}

Plugging it in, the board immediately began to play [*Für Elise*](https://www.youtube.com/watch?v=_mVW8tgGY_w)*, *which surprised me because I wasn't expecting anything other than maybe a tone to be played because of the simplicity of the components. However, on further examination I noticed that there was a chip labelled with **FT66T-19S,** which turned up a datasheet from a company named Fortech. I could not find the company anywhere else.

![](/images/2018/12/IMG_20181222_185214_HDR.jpg)

Searching for music melody chips online, there are a few companies selling chips [like this on](https://www.aliexpress.com/item/10pcs-lot-UM66T-19L-TO-92-ZL-66T-19L-ZL66T-19L-UM66T-19-UM66T-UM66T19L-Melody/32870372042.html?spm=2114.search0104.3.2.3429276667y12V&ws_ab_test=searchweb0_0,searchweb201602_4_10065_10068_10130_10890_10547_319_10546_317_10548_10545_10696_453_10084_454_10083_10618_10307_538_537_536_10059_10884_10887_100031_321_322_10103,searchweb201603_51,ppcSwitch_0&algo_expid=027de9a6-211c-4375-a94d-37073e51ae0f-0&algo_pvid=027de9a6-211c-4375-a94d-37073e51ae0f) AliExpress and Hong Kong websites. The labeling appears to be the same: the company's name the the first two initials, then 66T-XXL, where XX relates to the tone that is played. Some digging suggests that Fortech is the manufacturer of the chip, because [the datasheet](http://product.ic114.com/PDF/F/FT66T-xx.pdf) contains ordering and customization details, including mask options.

The operation is quite simple, as stated in [the datasheet](http://www.e-ele.net/DataSheet/UM66T-xxL.pdf) for an identical chip. There are three pins, one for power, one for ground, and one for the source, which drives a separate transistor that controls the operation of the speaker.

{{< figure src="/images/2018/12/IMG_20181222_185316_HDR.jpg" alt="" caption="Back of board" >}}

On the board that I have, the 100$\ohm$ resistor is in series with the LED, and the transistor on is connected on the low side of the speaker. The 1K resistor is connected to the source of the IC and to the gate of the transistor, and the last 100$\ohm$ resistor is connected to the 3.3V line, and acts as a current limiter for the 3.3V line on the board, because the button pulls the 3.3V line to ground to reset the FT66T.

{{< figure src="/images/2018/12/image-11.png" alt="" caption="Circuit" >}}

# Conclusion

Exploring this little board was fun. This circuit could also serve as a good introduction to soldering because most of it is through-hole and you get interesting feedback. One improvement that can be made is the use of a voltage regulator that could take a 9V battery, or a battery holder can be included in the kit. An interesting soldermask can also be applied for visual interest such as the example from Circuit Classics below.

{{< figure src="/images/2018/12/image-12.png" alt="" caption="A simple learning circuit." >}}
