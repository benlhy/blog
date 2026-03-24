---
title: "OnePlus One Fastboot fix"
date: 2023-03-02T14:48:20+00:00
slug: "oneplus-one-fastboot-fix"
draft: false
description: "I fixed my OnePlus's One fastboot driver by using the same driver that was recognised by Windows for ADB and reassigned to the detected fastboot device."
cover:
  image: "/images/2023/03/1-sandstone-black.png"
  relative: false
  hiddenInList: false
tag: ["oneplus", "fastboot", "Android", "error", "Windows", "fix", "magisk", "root"]
---

I own a second-hand OnePlus One and it is one of my favorite devices for Android development: ample RAM, a beautiful screen, light, and easy to grip. I use it as a testbed for Android and recently I had to flash Magisk on it.

The phone worked well until it was time to run `fastboot flash boot magisk...`

It reported `<waiting for any device>` and I realised that it wasn't being detected correctly when in fastboot mode because `fastboot device` would not return any connected devices. `adb` worked, so it wasn't an issue with my hardware. The solution is to point Windows 10/11 to the right driver and not:

- Install a driver from a sketchy website(!!!)
- Disable driver signing on Windows.
- Reinstalling the original ROM to get the drivers that only show up as a CD-ROM driver

Start in fastboot mode

{{< figure src="/images/2023/03/image.png" alt="" caption="Android will not be detected correctly" >}}

Browse My Computer for Drivers

![](/images/2023/03/image-1.png)

Let me pick from available drivers

![](/images/2023/03/image-2.png)

Select Universal Serial Bus devices

![](/images/2023/03/image-3.png)

Under WinUSB device select ADB device

![](/images/2023/03/image-4.png)

Accept the warning

![](/images/2023/03/image-5.png)

![](/images/2023/03/image-6.png)

Now it will show up in fastboot devices. This is because ADB uses the same drivers as fastboot, but when you boot in fastboot mode Windows may not detect the hardware correctly.

# References

- [https://community.oneplus.com/thread/804764](https://community.oneplus.com/thread/804764)
