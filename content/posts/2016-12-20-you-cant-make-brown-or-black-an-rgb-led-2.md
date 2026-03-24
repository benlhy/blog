---
title: "You can't make brown or black with an RGB LED (and other ended projects)"
date: 2016-12-20T02:41:57+00:00
slug: "you-cant-make-brown-or-black-an-rgb-led-2"
draft: false
tag: ["ended-projects"]
---

Not with LEDs. You can only make whatever color that is in the color wheel. That is to say only what you can get when you mix red or blue light, or blue with green. Or green with red. That is all.

![](http://www.vanseodesign.com/blog/wp-content/uploads/2010/02/colorwheel-rgb.jpg)

Figure 1. RGB color wheel

The best you can do is an orange. Darkening the orange only makes it look like a dim orange. The problem here is that you don't have black, which is what you really need if you're aspiring to represent all colors.

I found this out the hard way when I was trying to design a visual reference to resistors. The idea was that for each potentiometer knob, above would be an RGB LED that would represent the color of the resistor. You turn the knobs until the colors matched, and there would be an OLED screen that would tell you the value of the resistor.

###### Can't put a compass near a motor either

Motors generate magnetic fields. Compasses use magnetic fields to tell the direction. Robots use direction to plan their paths. Without direction, there can be no movement. The only solution is to mount the compass really far away (ugly) or to shield the motors (no cheap way).

I was planning to have an autonomous robot be able to navigate based on GPS and use the compass bearing to plan a route. However, I'll have to find a long pole to secure my compass somewhere further away from my motors to avoid having magnetic fields interfering with my readings.

###### Can't use Raspberry Pi with Adafruit Bluefruit either

I was pretty excited to get my Adabox, which contained a really cute robot. I was messing around with it and got a pretty great idea of having it chase a ball using computer vision. I wanted to use the Raspberry Pi zero (with the Red Bear IoT hat) with a camera to use image processing techniques to find the ball, but the problem is that the Pi's Bluetooth doesn't play nice with Adafruit's implementation. This is an existing problem that hasn't yet been fixed. I tried to investigate but the problem appears to be out of my depth. A solution would be to hardwire connections to the module, but that would just render the bluetooth useless.

###### Can't run a robot on unpacked snow

Not with wheels or tracks.

###### Shouldn't use makefile with the Teensy 3.6

Arduino works fine. It is not the 'easy' way out. Although I'd be the first to admit that it is far easier than configuring the environment to use `make`.

###### Having problems with pianobar? Yep, it is broken

I had so much trouble trying to install pianobar. You first get a TLS fingerprint error which is associated with an old TLS signature because the packages in the official repositories are *4 years old*. The best and simplest solution I found is Adafruit's tutorial: `https://learn.adafruit.com/pi-wifi-radio/raspberry-pi-setup-2-of-3`.

```
sudo apt-get install git i2c-tools python-pexpect python-smbus libavfilter-dev libavformat-dev libcurl4-gnutls-dev libgcrypt-dev libjson0-dev libao-dev

```

Once the libraries are installed pull the file from Github:

```
git clone https://github.com/PromyLOPh/pianobar

```

Run the following commands to compile pianobar and make a blank configuration file:

```
cd pianobar
make
sudo cp pianobar /usr/local/bin
cd
mkdir .config/pianobar/
cd .config/pianobar/
sudo nano config

```

Use Adafruit's configuration file: [https://github.com/adafruit/Python-WiFi-Radio/blob/master/config](https://github.com/adafruit/Python-WiFi-Radio/blob/master/config)

Remember to change the password and email. And you can start pianobar from the command line!
