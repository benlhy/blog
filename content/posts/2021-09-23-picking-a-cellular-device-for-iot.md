---
title: "Picking a cellular module for IoT"
date: 2021-09-23T23:36:01+00:00
slug: "picking-a-cellular-device-for-iot"
draft: false
description: "A collection of cellular devices and selection considerations for IoT"
cover:
  image: "/images/2021/09/igor-6mFcQOOi5QY-unsplash-2.jpg"
  relative: false
  hiddenInList: false
tags: ["IoT", "development", "2020", "Cellular", "Nordic", "Quectel", "Sierra", "IC", "module"]
---

In my quest to achieve True Wireless™, I recently got interested in cellular modules because they provide the ultimate in wireless connectivity - there are no modems and no routers required.

I thought searching for a cellular module would be like any other wireless technology that I'm familiar with: start from the hobby-level products from Adafruit and Sparkfun and gradually work my way backwards to discover what are the best cellular modules based on observed usage in other products. This method saves me a lot of time and effort in narrowing down the selection choice.

To my surprise, it turns out that the cellular module space isn't that crowded at all. For example, Nordic Semiconductor, which makes some of the most popular Bluetooth ICs in the market, with many third-party modules and is generally regarded as a wireless specialist, only has one cellular module.

u-blox is another big player in the wireless space and even then, they have only one or two popular modules that other companies use.

And from there we go to the biggest players like Sierra Wireless, Qualcomm, and Quectel. These are large industrial companies that build wireless SoCs for smartphones. As an individual developer, it isn't practical to get started as they require large minimum order counts, have expensive development kits, and are very development intensive on the software side of things.

# Key considerations

**Official Support** - this is the biggest factor for me when selecting anything. It doesn't matter if it is the smallest, fastest, cheapest module on the planet. If it is hard to get support, I would look elsewhere in 90% of scenarios. You might be able to get dedicated support if you are a large company, but how a company treats it's smallest customers is a good indicator of the support you'll get.

**Forum Support** - Sometimes companies use forums as a knowledge management system. The activity, as well as the presence of actual engineers from the company, are important indicators of how much support you can get in this space. Generally Texas Instruments and Nordic has done this quite well.

**Quality Documentation **- With good documentation, there is no need to reach for support, and if the documentation is bad, even if the support is world-class, it slows down the development process because you have to consult support at each step.

**Community** - The size of community is important because you'll find people who might have the expertise to solve your problem, or they have solved your problem before. The bigger the community, the faster you'll get responses.

**SIM availability** - Even today I'm still having difficulties obtaining CAT-M1 or NB-IoT SIMs from my cellular service provider because these are mostly catered towards enterprise customers. A module with a globally pre-provisioned SIM card and data plan makes it much smoother to develop applications.

# Cellular Modules

## Particle.io
{{< bookmark
  url="https://store.particle.io/collections/ethersim/products/boron-2g-3g-global-starter-kit-ethersim"
  title="Boron 2G/3G Global Starter Kit with Ether"
  description="SIMThis product is powered by EtherSIM, Particle's new globally available, self-optimizing SIM with no data plans or device fees. Buy this hardware and start building with Particle's Free Plan which includes support for up to 100 devices, cellular data, and fully-featured platform access with no time l…"
  favicon="https://cdn.shopify.com/s/files/1/0925/6626/t/100/assets/favicon.ico?v&#x3D;5332671134818864896"
  site="Particle Retail"
  author="Particle"
>}}
✅ The Boron line is what I was looking for. They are using Quectel modules under the hood. The fact that they have M.2 modules is great too as it means that it may be easier to design for production.

✅ Free* data plan! (Limited to 100MB of cellular data a month pooled across all devices)

😐 No response on forums - support is the most important factor when it comes to designing with new devices. You don't want to work on something for weeks and then get stuck and be unable to progress because there is no support for a product. [Edit: 18 Feb 2022] This seems to have improved with most questions on the forums being replied to by a staff member.

❌ Limited to 2G/3G globally and no ability to interface with 3rd party SIM cards. Global M.2 cards do not exist at time of writing.

## Blues Wireless
{{< bookmark
  url="https://blues.io/products/notecard/"
  title="Notecard · Blues Wireless"
  description="Blues Wireless makes wireless IoT simple with the Notecard and Notehub."
  favicon="https://blues.io/images/favicon/apple-touch-icon.png"
  site="We're making Massive Io"
  author="T implementation quick and easy. Start your journey with us! Get Started Contact Sales"
>}}
✅ Has a full API for controlling the module from a controller MCU over UART and $I^2$C. Not your standard AT command set, but this has allowed them to create a UART-over-I$^2$C if you ever need more UART pins.

✅ Supports micropython, circuitpython, C and JS!

✅ Comes in a handy m.2 format - easy to integrate into existing designs

✅ Included 10 year, 500MB contract! I feel that this may change in the future, but for now, that's a great deal for the price of the card and the data plan.

✅ Active community engagement on Hackster.io - projects published periodically by the team.

## Sierra Wireless

😐 HL is their legacy series of cellular modules. WP is their new series. Took me awhile to find that out. I prefer to have a company point me to their latest product instead of trying to infer it from when the documentation was last updated.

❌ Legato application platform - looks complicated to write an app.

❌ MangOH yellow dev kit - great! But the video and channel uses a robot voiceover and it only has a few hundred views. I feel that the credibility takes a hit when a robot voice is used, simply because most poor quality videos use the same voiceovers.

❌ Small community - this would be challenging to drive development

## u-blox

✅ Seems to be the most popular cellular device on the block with multiple companies using versions of it on their devices: Digi Xbee, Sparkfun Micromod Asset Tracker

✅ The Sara-R4 series seems to be the most popular module in general for hobby-level use.

❌ Sara-R5 is the next gen IC, but there appears to be some issues sourcing the modules and the evaluation kit EVK.

❌ Expensive evaluation kit - $500 USD compared to around $200 USD for the Nordic nRF9160 development kit.

## Digi XBee3
[Digi XBee 3 Cellular LTE-M/NB-IoT modemCompact, flexible cellular connectivity for IoT devices and gateways![](https://www.digi.com/images/apple-icon.png)Digi![](https://www.digi.com/products/assets/digi-xbee-3-cellular/digi-xbee3-lte-m-nb-iot)](https://www.digi.com/products/embedded-systems/digi-xbee/cellular-modems/xbee3-cellular-lte-m-nb-iot)
✅ Common footprint - Xbee style

✅ Available in micropython! This is a huge plus as python allows you to get projects up and working easily.

✅ This appears to use u-blox technology as well

❌ Unclear data plan.

## Nordic

✅ Nordic has a ton of experience building wireless devices.

✅ Variety of modules and dev kits available: m.2 (Fastel), click module (Mikro), and breakout board (Sparkfun)

✅ Great support with plenty of people using the modules

❌ Does not come with an inbuilt SIM or pre-provisioned SIM card.

😐 However this is their one and only cellular IC: nRF9160. Could mean that they are focused, or also could mean that they are not very experienced yet.

## 

---

These are not technically cellular modules, but they fulfill roughly the same purpose - getting data over long range.

## Swarm

- Uses their own satellite network - good: because they are cheaper, bad: what happens when they go out of business? They are new, so only time will tell if they will be around in the next few years.
- Not readily available yet.
- Promises to be quite cost effective as well - \$119 for a module/\$5 a month, 750 data packets/month, 192 bytes/packet subscription.

## Iridium

- Expensive! Modules go for \$250 ea, \$15 a month subscription, 1 credit/50 bytes, \$0.13/credit.
- Resold through Rockblock
- Has been around for a long long time.

Item
Swarm
Iridium

Module
$119
$250

Line cost
$5
$15

Data per month
114KB
114KB

Cost per month
0
$160

Total
$124
$425

# Conclusion

After taking awhile to do some exploration, I decided that Blues Wireless was the best bet for me to get started. They have developer friendly documentation, as well as a path to scaling. Since my use cases doesn't extend to areas with no cell coverage, I stuck with CAT-M1 and NB-IoT implementations.
