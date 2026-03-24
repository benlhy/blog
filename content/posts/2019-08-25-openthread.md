---
title: "OpenThread"
date: 2019-08-25T15:20:12+00:00
slug: "openthread"
draft: false
description: "An overview and implementation of OpenThread, a mesh networking protocol in BLE 5.0. Featuring the Nordic nRF52840! Fun and very accessible."
cover:
  image: "/images/2019/08/ot-icon-toprow-ot_480.png"
  relative: false
  hiddenInList: false
tags: ["OpenThread", "Google", "5.0", "Microsoft", "BLE", "Nordic", "nRF52840", "Raspberry Pi"]
---

# What is Thread?

Thread is a self-healing mesh network that uses the same radio as Bluetooth 5.0. It supports external commissioning, encrypted communications by default, and is simple to set up.

The great thing about Thread is that a lot of the low level addressing and networking is already done for you, so you are free to implement your network quickly and easily instead of futzing around with configurations.

## How to get started

My recommendation is to set up a Raspberry Pi Border Router. An alternative is to [use the Raspi image that Nordic has already set up](https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5-SDK-for-Thread/RaspPiOTBorderRouterDemov3101alpha.zip), but I ran into a few issues booting the Raspberry Pi with the image, so [I decided to go with the docker image that Google has provided](https://openthread.io/guides/border-router/docker).

These instructions assume that you have never worked with Nordic devices before. I highly recommend using Nordic devices for experimenting with Thread. They are quite affordable, have a good SDK, and have excellent support via their forums (actual engineers are assigned to your questions). Plus the [codelabs](https://codelabs.developers.google.com/codelabs/openthread-hardware) (which are really helpful!) that OpenThread provides are used with nRF devices.

### Required items

- Raspberry Pi 3
- nRF52840 dongle
- 2x nRF52840 DK

### Steps

- Set up the Raspberry Pi. You can use either the lite or the full image.
- At the same time download Segger Embedded Studio register it for Nordic.
- Download the nRF Connect and the nRF SDK for Mesh and Zigbee.
- Flash the dongle (PCA10059) with the NCP hex from the SDK using nRF Connect. Attach the dongle to the Raspberry Pi.
- Download the docker image for the OpenThread Border Router (OTBR) for the Raspberry Pi and set it up. Form the Thread network.
- Flash the CLI hex to the nRF52840 DK (PCA10056).
- Using the CLI, with a Baud rate of 115200, configure and save the dataset for the Thread network. In this case we will provide the master key directly to the joining device, so no commissioning is required.
- Once you connect the DK's state should read `router` and not `leader`

### Additional Steps for MQTT-SN

- Compile the paho MQTT-SN from source
- Install Mosquitto
- Configure MQTT-SN to connect to the local broker

## Additional Reading

The OpenThread codelabs are actually quite useful in going over the API for OpenThread. In this regard, Nordic's SDK is a little lacking in detail on how to modify and configure the examples. Once you are done with the Thread Primer and understand how OpenThread works, I recommend going over the codelabs to get a good feel of how to code works.

- [https://openthread.io/guides](https://openthread.io/guides)

### Recommended Tasks

- Configure your device to join a network on start instead of using the command line to configure the dataset.

# Setting up a Thread Device

This is the minimal setup for a Thread Device, assuming that you already have a Border Router set up and a Thread network formed.

```
dataset masterkey *
dataset panid 0x*
dataset commit active
ifconfig up
thread start
```

## Testing connectivity

These are a few commands to test the connectivity of the device.

```
status
router table
child list
ping 64:ff9b::acd9:a46e
```

# Glossary

- MQTTSN - MQTT for Sensor Networks
- FTD - Full Thread Device
- MTD - Minimal Thread Device. Only acts as a end device.
- NCP - Network Co-Processor
- REED - Router Eligible End Device. This device can be promoted to a router if the situations demands it.
- MLE - Mesh Link Establishment message
- CoAP - Constrained Application Protocol (used for mesh-local/multicast messages)
- PAN - Personal Area Network

# IPv6 Addressing

### fe80::/16

Link-Local Address, used to discover neighbors and configure links. **Not a routable address.**

### fd00::/8

Mesh-Local address, should be used by applications for routing.

### 0000:00ff:fe00:RLOC16

Routing locator, identifies a thread interface, based on location in network topology. Changes as the network changes.

### ff01::1

All FTD and MEDs, Link-Local

### ff02::2

All FTDs, Link-Local

### ff03::1

All FTD and MEDs, Mesh-Local

### ff03:2

All FTDs, Mesh-Local

## 

# CoAP

Uses a binary RESTful protocol with four methods: POST, GET, PUT, DELETE

# Thread Size Limits

Thread supports:

- 1 Leader
- 32 Routers
- 511 End devices per router

This means each Thread network can have up to 16000 devices. I guess if you have more devices than that it might make more sense to create another Thread network.
