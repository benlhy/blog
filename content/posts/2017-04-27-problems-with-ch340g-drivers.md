---
title: "Problems with CH340G drivers"
date: 2017-04-27T23:05:20+00:00
slug: "problems-with-ch340g-drivers"
draft: false
tag: ["ulcek", "wemos", "Arduino", "Guides"]
---

So some cheap Ardunino Nano and the WeMos D1 Mini use the CH340G drivers. This has caused some users that are on Yosemite or later to experience persistent rebooting issues.

The current fix is to use a newer version of the WeMos D1 Mini, ie the WeMos D1 mini Pro that uses a CP2104 driver to communicate with the computer.
