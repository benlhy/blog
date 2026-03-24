---
title: "Achieving Ultra Low Power Bluetooth with BG22"
date: 2020-12-12T02:44:00+00:00
slug: "ultra-low-power-bg22"
draft: false
description: "How to get the lowest power consumption on the BG22 using project defaults. Or how to get 100,000 hours runtime on a BG22. "
cover:
  image: "/images/2020/11/battery2.JPG"
  relative: false
  hiddenInList: false
tags: ["BG22", "Silicon Labs", "2020", "Bluetooth", "BLE", "low power"]
---

When a company claims that I can get 1uA power consumption in a standby state for a wireless device, I have to test that claim.

1uA effectively means 100,000 hours on a single CR2032 coin cell battery. And 100,000 hours means 10 years of running on a single coin cell.

Compare against Nordic's excellent nRF52840. It is very popular amongst hobbyists, and it has seen some inroads into commercial products due to its low power features, but the current draw when transmitting under the same conditions as the BG22 (0 dBm, DCDC) draws 1.5x more current (6.40mA vs 4.4mA ). Another popular choice for BLE is the CC2520 from Texas Instruments which I've seen featured amongst many commercial Bluetooth products: beacons, dongles, exercise bands... you name it, it probably has a CC2520 or some variant of that inside. But it doesn't do 1uA of current

For now, we will dive straight into testing out the BG22's low power claims.

For this test, I am using the BG22 Thunderboard Kit attached to a WSTK for current sensing.

---

# Diving in

When the BG22 is awake and broadcasting, it draws 1 ~ 3 mA, which is about 1000x more than 1uA. So if we spend all of our time sleeping, we can effectively extend the life of the system by a *significant amount.*

However, it is not a very interesting test if the chip just sits there and does nothing. So let's have the BG22 do something a little more exciting:
{{< bookmark
  url="https://westsideelectronics.com/better-cheaper-soil-sensor/"
  title="Better Soil Sensor"
  description="How a capacitive moisture sensor works with a 555 timer, and getting it to work with the BG22 Bluetooth chip from Silicon Labs!"
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
I decided to bring back the soil sensor application - it will activate the ADC, read a few values, and then go back to sleep. For this test I've decided to just test the BGM220 module, which a module-based off the BG22. I've disconnected the soil sensor which draws around 8mA when on to focus solely on the current draw of the BG22.

## Trouble

When I uploaded the code for the soil sensor application, I immediately ran into the first issue where the chip was **drawing about 1mA on average**, even when the soil sensor was disconnected. I wasn't too concerned initially, because I haven't optimize for the power saving features in my other project yet.

However, when I dived deeper into the code, I realized that the Power Manager software module was already installed in the default *empty-SoC Bluetooth* application. It automatically handles entering and leaving of all the power saving states from EM0 (active) to EM3 (deep sleep).

So I was already using the power saving features without being aware of it. So why was I drawing about 1mA? I made very sure to shut off all external peripherals, even the ADC when the device goes to sleep, and I even tried to manually toggle the EM3 state myself, but I was still coming up short because the power consumption stubbornly remained at 1mA.

In a fit of frustration, I decided to create the base template from scratch and that was where I finally discovered my mistake:

> ***I forgot to turn on power saving features for USART!* **

I neglected to turn on the power saving features on the USART peripheral. Honestly this wasn't a documented feature and I feel a little cheated.

![](/images/2020/11/image-22.png)

Without toggling the restrict energy mode, the module draws about 1mA staying at EM0. However, when it stops broadcasting and turns off all peripherals, it can actually go all the way down to 97nA. 😲

> There is still a 1mA glitch with the WSTK and SSv5 where a series 2 IC will draw 1mA when it is just flashed or the secure bootloader is read. To fix this, the IC just has to be reset for it to start drawing the right amount of power. There is an additional issue where the chip will seem to draw 80uA at minimum, but this can be handled by updating the WSTK.

{{< figure src="/images/2020/11/image-21.png" alt="" caption="Module going from sleep to broadcasting" >}}

Overall, with a power scheme of being 50% on and 50% sleeping, the module draws around 248uA on average. The current and power draw can definitely be brought down even lower by increasing the percentage of time spent sleeping. In fact for an operation like sampling environmental data, this can be done for a few seconds every 10 minutes (which might even be excessive). This presents a ratio of $$\frac{I_{\text{active}}}{I_{\text{total}}} = \frac{10}{10*3600} = \frac{1}{3600}$$

Previously the duty cycle was:

$$ \frac{I_{\text{active}}}{I_{\text{total}}} = \frac{1}{2} = \frac{1800}{3600}$$

Which implies that

$$I_{\text{new}} = \frac{1}{1800} \times 248 = 0.13 \text{uA}$$

This implies that we can theoretically bring down the current usage from 248uA to 0.13uA, but realistically I would peg it to 1uA.

For a CR2032 battery, which has a capacity of about 100mAh before the voltage starts to drop precipitously, this means the application can run for 403 h at maximum, which is roughly two weeks of continous broadcasting. Pushing it to the extreme of hourly reporting of a period of 3 seconds yields 600x increase in the lifespan - roughly 5 years.

That is quite a long time for a coin cell battery, but how would it perform when paired up to **Infinite Energy from the Sun™**? Will it last a month? A year? Well, the only way to find out is to test it!

So instead of a tiny CR2032 battery, I hooked it up to the fairy light that boasts a 800mAh battery. This effectively gives us a month of operation just off the battery alone. Coupled with trickle charging the battery with the solar panel, it gives us a *very* long run time.
