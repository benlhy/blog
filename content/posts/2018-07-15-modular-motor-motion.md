---
title: "Motor Motion Module"
date: 2018-07-15T01:48:09+00:00
slug: "modular-motor-motion"
draft: false
cover:
  image: "/images/2018/04/rebus.jpg"
  relative: false
  hiddenInList: false
---

This is a DC Motor Controller that is built off the TIVA TM4C123. This microcontroller has several nice features, such as a CAN bus and two hardware quadrature decoders. This means that we can attach quadrature encoders to any motor and use them to compute a position for the motor. With a CAN bus we can link up multiple controllers together to precisely control the motion of each motor. This enables the builder to construct bigger and more elaborate robots with each connection. For example, a single board can drive a differential drive robot. Two boards can drive a SCARA arm robot, three boards can drive a full 6DOF robot arm.

![solderedBoard](/images/2018/04/solderedBoard.JPG)

The idea for this board comes from [Hebi Robotics](http://hebirobotics.com/products/).

Additionally, a Launchpad Boost Board has also been developed.

![boost](/images/2018/04/boost.JPG)

The [boards](https://github.com/benlhy/DC-motor-control) and the [code](https://github.com/benlhy/DC-Control-Module) are avaliable on Github.

# Why use DC motors?

There are a few alternatives to using DC motors. There are stepper motors, Brushless DC motors (BLDC motors), and servo motors. The advantage of DC motors is that they are relatively simple and and cheap. By decoupling the quadrature encoder, you can easily add your own in order to obtain the best performance.

Different controllers can also be programmed into these motors through the microcontroller to allow for a range such as PD, PID, or lead-lag control. This allows for very precise tuning for any application that is not avaliable on servo and brushless motors.

# How can it be used?

The boards can be hooked up to each other and they communicate over a CAN bus. Each board can be assigned an ID, and the user can program which order the IDs are in, or an additional wire can be added and each board will be able to self configure their location on the chain.

The boards are controlled from a computer with a UART interface.

Programming is done through Code Composer Studio. I like Code Composer Studio because the IDE is pretty simple to use and it just works with Launchpad hardware.
