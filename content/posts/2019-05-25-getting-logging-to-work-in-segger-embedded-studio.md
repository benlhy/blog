---
title: "Getting NRF Logging to work in Segger Embedded Studio"
date: 2019-05-25T14:15:08+00:00
slug: "getting-logging-to-work-in-segger-embedded-studio"
draft: false
description: "Getting NRF logging to work in Segger Embedded Studio"
cover:
  image: "/images/2019/05/Capture.JPG"
  relative: false
  hiddenInList: false
tag: ["Segger", "RTT", "Logging", "classes", "nrf52", "Nordic", "tips"]
---

Segger Embedded Studio is a lightweight IDE that is used for Nordic devices. Getting logging to work with the 15.3 SDK has been a long journey though, so I thought I might write down my process in getting the debug terminal to print out RTT messages.

## Ensure flags have been set

Ensure that RTT logging has been enabled in sdk_config.h:

`#define NRF_LOG_BACKEND_RTT_ENABLED 1`

and

`#define NRF_LOG_ENABLED 1`

## Use the correct sdk_config.h

For some reason the example code for the TWI scanner did not work for me, and copy-pasting code from TWI sensor for the `sdk_config.h` file worked. Check by launching Segger RTT viewer. If the flags have been set and the correct config file is used, then you should start to see some output.

## No output on the debug terminal?

Close the RTT viewer since you can only have one RTT viewer open at any point in time.

## New lines printed on the debug terminal but no text?

This is actually a bug with the SDK. Check it out [here](https://devzone.nordicsemi.com/f/nordic-q-a/45985/nrf_log-not-working-on-segger-embedded-studio) on the Nordic forums, but essentially you have to set this line to 0 in sdk_config.h

`#define NRF_FPRINTF_FLAG_AUTOMATIC_CR_ON_LF_ENABLED 0`

{{< figure src="/images/2019/05/image.png" alt="" caption="Output!" >}}
