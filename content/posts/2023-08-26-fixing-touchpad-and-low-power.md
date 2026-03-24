---
title: "Calibration for Touch Sensitive Devices"
date: 2023-08-26T10:45:14+00:00
slug: "fixing-touchpad-and-low-power"
draft: false
description: "Making the headphones more reliable and exploring low power options on the PFS173."
cover:
  image: "/images/2022/12/Untitled.png"
  relative: false
  hiddenInList: false
tag: ["$0.03", "3 cent microcontroller", "2022", "Mini-C", "Touch", "programming", "Padauk", "low power"]
---

After modding my headphones, I realized that the touch capacitance was not as sensitive as I required it to be. Because of the tight timing requirements to detect if a touch is present, small changes in the environment can result in large changes in the detected delay.

It was hard to debug because Padauk does not have any debugging hardware on the IC itself, relying instead on emulation for programming and debugging. However, emulation means that it is unable to replicate the timing and chip performance perfectly, which adds another layer of delay and uncertainty when programming the threshold for detecting touch.

After a few frustrating rounds of programming, desoldering, and resoldering, I decided that I needed a calibration routine that would set a baseline for the IC.

{{< figure src="/images/2022/12/image-3.png" alt="" caption="Mini-C in all its glory" >}}

The calibration sets each pin on the microcontroller to high for a long (relatively) time to stablise the reading, and quickly sets the pin to an input. This allows the externally connected capacitor to discharge and we will wait until the pin detects a low signal. This is averaged over 8 times to get an accurate baseline and worked like a charm. After this baselining code was put in place, touch worked accurately every time.

# Power Savings

Since the headphones use a relatively small battery of about 500mAh, there is also a need to conserve power as the touchpad works by charging and discharging power. Without any power optimization, the current draw is 654uA.

![](/images/2022/11/image-4.png)

However if we stop execution of the main clock, effectively pausing the device, we can achieve 2.5uA of power draw.

{{< figure src="/images/2022/12/image-2.png" alt="" caption="STOPEXE is to stop execution" >}}

Technically, it is also possible to go lower by switching the timer to a low frequency clock, but I found that was problematic as the touch sensitivity would be affected. A simple test showed that the Padauk datasheet matched up to the real world measurements.

![](/images/2022/11/image-5.png)

An active:sleep ratio of about 1:10 represents power savings of about 90%. Since the sleep current is negligible compared to the active current, the power savings are proportional to the time spent sleeping.

![](/images/2022/11/image-11.png)

Implementing the power saving features in the actual touchpad results in the sleep power draw of 9uA, significantly less than the active draw of 600uA.

I find that the headphones have an effective life of about 2-3 days, as most of the power draw comes from maintaining the BLE connection and powering the speakers. However, this was instructive in exploring the low power options of the Padauk PFS173 as well as some of the more interesting aspects of Mini-C like variable interpolation within a loop. As a language, Mini-C is a very limited dialect, but it is very effective for what it needs to do.
