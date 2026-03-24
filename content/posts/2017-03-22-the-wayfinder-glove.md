---
title: "The Wayfinder Glove"
date: 2017-03-22T17:00:28+00:00
slug: "the-wayfinder-glove"
draft: false
cover:
  image: "/images/2017/08/IMG_0226.JPG"
  relative: false
  hiddenInList: false
tags: ["Projects", "wayfinder glove"]
---

This is a cycling glove that lights up with a flick of the wrist. Additional projected capabilities include bluetooth communication, a compass mode, and a waypoint mode where the glove points you to your next waypoint.

### Lights

![](/images/2017/03/glove-2.jpg)

Figure 1. Lighting on glove

This glove uses the Adafruit Playground as the board and it is powered by two CR2032 batteries.

The first of these capabilities is the lighting on the glove. It is meant to provide light to show you the way, and the Neopixels are specifically positioned to point forward when you clench your fist. This is useful when you want to hold something and need a light.

![](/images/2017/08/IMG_0226-1.JPG)

Figure 2. Grip

I initially thought about putting the lights in the palm. While it would look really cool to have light coming out from your palms after your flick your wrists, this is quite impractical because holding your wrist up and pointing forward is an awkward position to hold for an extended period of time, and you won't be able to hold anything with the hand.

One challenge of getting this working is that the measured change in acceleration has to detect that it is a flick and not just my hand moving around. For this I decided on using a difference in the previous value and the next value obtained by the sensor to tell if I had flicked my wrist or not.

![](/images/2017/03/output.gif)

Figure 3. Working flick-to-light mode

I chose the Adafruit Circuit Playground because it has a variety of sensors on board with ten neopixels on the board itself. It has a a lipo battery charger which makes it wonderfully easy to power on the go. It also has two user buttons, which allowed me to change the state of the program if I so desired. This will be useful in changing modes or holding modes.

However, it turned out that the buttons do not support interrupts, and the compass I decided to add on was acting strangely so I took it off. It is still quite a cool glove that lights up upon shaking.

### Compass and Accelerometer (7 May 2017)

After my success with the M0+ breakout board, I managed to prototype a working board based on the Adafruit Flora board, but with a M0+ as it's main chip. It is quite an overkill to use it, and in hindsight I might have been able to use a Attiny85 instead, but the M0+ can be used in a variety of other projects, so it is a investment for the future when I might want to do more advanced wearable projects.

![](/images/2017/05/wearable.jpg)

It incorporates a reset button, a switch, and a LSM303 compass+accelerometer sensor in place of the WS2812 LED. It also features a charlieplexed array of 8 LEDs and a SWD interface for programming. USB is also included because the M0+ is able to support virtual USB for uploading code.

I am calling this the **Wayfinder One** glove.

However, one issue that I have with this board is that the reset button is too close to the compass, causing the LSM303DHLC compass to give weird readings until I removed the reset button. My supposition is that the metal on the reset button affects the detected magnetic fields.

![](/images/2017/05/crop.jpg)

In the image above you can see the removed reset button (amongst other components I removed). I used the pololu code for a tilt-compensated compass and the charlieplexed LEDs were able to point to the North quite consistently. In my next design, I plan to move the reset button above the programming headers.
