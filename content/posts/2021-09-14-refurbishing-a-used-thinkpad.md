---
title: "How to get a good computer for cheap"
date: 2021-09-14T04:00:53+00:00
slug: "refurbishing-a-used-thinkpad"
draft: false
description: "How to get a high-quality Windows development machine on the cheap. Or how I tricked out my Thinkpad from Ebay."
cover:
  image: "/images/2021/09/template-1--1.jpg"
  relative: false
  hiddenInList: false
tags: ["2021", "cheap", "computer", "laptop", "refurbish", "Lenovo", "Thinkpad", "X1", "Carbon", "low-cost", "easy", "less than $300", "price"]
---

*Why spend \$300 to \$500 on a mediocre, mid-range laptop or desktop when you can have a highly portable, powerful Windows machine? Whether it is looking for yourself or for a loved one, consider a used Thinkpad. They regularly come onto the used market for cheap as companies renew their stock of Thinkpads, and they offer a very good price to performance ratio. Because they are older, you can also find old and used peripherals to trick out the Thinkpad for a song.*

---

# Tldr

I got a tricked out Thinkpad for < \$300 dollars. It has an NVMe SSD, a dock, and even an LTE module so I can work from anywhere.

- [Bobble.Tech](https://www.bobble.tech/free-stuff/used-thinkpad-buyers-guide) is a good reference for used X1 Carbons, and [DankPads](https://dankpads.com/tpg/) is another good reference for the features and prices of newer laptops.
- Find a local used-goods dealer. Ebay, Facebook Marketplace (too many scams), Carousell
- Wait 2 weeks while comparing prices.
- Buy the laptop.
- (Optional) Upgrade laptop with better functions.

# Background

I recently needed to do development on a Windows laptop because Mac did not support 32 bit applications, and the compiler that I am using needs to run as a 32 bit application (I'm looking at you KEIL).  I set my budget to $200, and started to look around, but all I could find in that range for new laptops were Chromebooks or woefully underpowered Intel Atom laptops.

But I was first cued into some labor day discounts on HP's website, and from there, my search and grand budget led me to EBay where I came across a familiar name: the X1 Carbon.

{{< figure src="/images/2021/09/image-11.png" alt="" caption="Re-enactment: This is actually quite a good deal for a 3rd Gen Carbon" >}}

I always liked the idea of owning the X1 Carbon - it has all the build characteristics of a Macbook Air, but with a Thinkpad keyboard and a Windows OS. However, the prices for X1 Carbons are always stratospheric as they are targeted at the luxury business end of the market, so I never got to own one. Exhibit A:

{{< figure src="/images/2021/09/image-9.png" alt="" caption="Ignore the sales price" >}}

Even with heavy discounts, it is still out of my price range.

Therefore, I began to dive into comparing various used X1 carbons: [Bobble.Tech](https://www.bobble.tech/free-stuff/used-thinkpad-buyers-guide) is quite a good reference for used X1 Carbons, and [DankPads](https://dankpads.com/tpg/) provides a good reference for the features and prices of newer laptops.
[Used ThinkPad Buyer’s GuideNew, live technology Q&A weekly on YouTube! Email your questions to robbie@bobble.tech, and join live or view the archive for the answers!![](https://lh5.googleusercontent.com/YsBsjFjmuRT8ANxby0cr9Y7fmzb3if6SWsxo9EPwi977NC70__c6u7r9lwKFNkMgB0nTDKNc7281mM-oj-IMCCkLR2UFklw)![](https://lh4.googleusercontent.com/MZ50o1d2QfzxDr7MdMGVtQ-Z9qFUb6iCFfbYmblw7tgGZiBuH2FXzYq3vWC_xOiToef6CA&#x3D;w16383)](https://www.bobble.tech/free-stuff/used-thinkpad-buyers-guide)Bobble.Tech[ThinkPad Price Guide V4.1Prices reflect July thru August 2020All are USD prices and are based off eBay sales in America.There are always higher priced units for sale on eBay, so wait for them on auction or give some best o…![](https://i1.wp.com/dankpads.com/wp-content/uploads/2018/09/cropped-E220s.jpg?fit&#x3D;192%2C192&ssl&#x3D;1)DankPads![](https://dankpads.com/wp-content/uploads/2020/08/V4-2-PNGversion-2800x4060-1.png)](https://dankpads.com/tpg/)DankPads Price guide
Things to look out for when buying the laptop:

- No BIOS password
- Those that don't turn on
- No picture with their screens on.

By and by I came into possession of a X1 Carbon through Ebay. While sellers often say that the battery life is not guaranteed and the system doesn't come with an OS, the laptop I received had a good battery life ~ 80% of its original capacity, and the OS license was tied to the laptop, so I installed Windows 10 using the Windows Media Creation Tool and the licence was automatically activated - sweet.

At this point if you are buying for someone else who doesn't have stringent requirements, you are done. However, if the machine you bought didn't come with good specs, read on to find out how to upgrade it!

The back of the X1 Carbon is held by Philips head screws, which makes it really easy to open up. Usually the two most important and easiest things to upgrade is the SSD and the RAM. In my case the RAM was soldered into the X1 Carbon, so I only upgraded the SSD.

# Upgrading the SSD

When I started to install the Windows operating system and all my other applications, it was clear that the 128GB stick of memory in my laptop would not be enough. By the time I was done, I was left with 60 GB of memory.

Checking the current specs of the laptop in [Lenovo's Warranty Lookup](https://pcsupport.lenovo.com/se/en/warrantylookup#/) revealed that the current drive was a SATA m.2 2280 drive. 2280 refers to the size of the m.2 drive (22mm x 80mm), and there are other sizes too, such as 2242 (22mm by 42mm) and 2260 (22mm x 60mm).

Since my Thinkpad was capable of reading NVMe drives which are faster than SATA drives, I decided to upgrade to a Crucial 500GB NVMe m.2 drive.

I wanted to clone my drive to the new SSD and replace the one in the computer, so then I would have a working laptop with significantly more SSD space without having to install everything from scratch.

To do that, I had to buy a NVMe/SATA enclosure, and based on my research, you might want to find one that sports the RealTek chip. It doesn't have thermal issues that might cripple the precious SSD that you put in there. In my experience, the case that I bought never got hot.

The m.2 port is keyed so that you don't make the mistake of putting the wrong drive into the wrong interface, but practically I didn't find this useful because even though some enclosures are keyed for B+M keys (which means that they should accept SATA and NVMe), they still tell you not to put NVMe devices in.

The final hiccup to the cloning process (I used Acronis True Image for Crucial) was that the PC complained about an inoperable boot drive when I swapped in my cloned boot drive. The steps I went through to fix it are:

- Change the original drive (SATA) from a MBR to a GPT partitioned drive so that it will work for an NVMe drive using the mbr2gpt tool in Windows.
- Re-clone original drive (SATA) to new drive (NVMe)
- Enter boot bios settings and select the Windows Boot Manager entry without the "(cloned)" postfix.

# Poor Trackpoint performance

The trackpoint is a crucial tool if you work on the go a lot and you need precision: for example, selecting text boxes or positioning elements in Powerpoint. Sometimes the trackpad just doesn't cut it.

A major bugbear for me was the poor trackpoint performance on the used Thinkpad. Despite turning the sensitivity up to maximum, it felt more sluggish than I was used to, and I had to really shove the trackpoint around to move the pointer. I noticed that the trackpoint that it came with was softer than I remember it to be and I happened to have another Thinkpad I could compare it's performance to and it was quite poor. I read from my searches is that the trackpoint's performance can degrade over time because of the softening of the plastic. The fix is simple: buy a new nub.

A useful link to check if certain parts fit your model is:  [Lenovo's Accessory Smart Find](https://accessorysmartfind.lenovo.com/#/products/4XH0X88960) which was where I learned that there are three different versions of the trackpoint. I found one that fit my device, and it worked perfectly - I even had to reduce the sensitivity back to the default setting.

# Enhanced function keys (Hotkeys) not working

{{< figure src="/images/2021/09/image-12.png" alt="" caption="Enhanced function keys" >}}

I use the enhanced function keys more than I use the function keys, they are the little symbols on the F1-F12 row. However, when I installed Windows, only the sound and the brightness functions worked.

In order to get it to work, I had to install the Hotkey Features Integration from Lenovo. Side note: Sharpkeys nor AutoHotKey can't detect these enhanced function keys until this update is installed.
{{< bookmark
  url="https://support.lenovo.com/us/en/downloads/ds120449-hotkey-features-integration-for-windows-10-64-bit-thinkpad"
  title="Hotkey Features Integration (Version 9) for Windows 10 (64-bit) - Think"
  description="Pad - Lenovo Support NL"
  favicon="https://support.lenovo.com/esv4/images/l-favicon.ico"
  site="Lenovo Support NL"
>}}
# Bluetooth

The Bluetooth wasn't working out of the box when I installed Windows 10 and running the troubleshooter would only give me the unhelpful result that "This device does not have Bluetooth."

Fortunately, Lenovo provides an archive of drivers under their [PC support page,](https://pcsupport.lenovo.com/us/en/) and installing the driver under the Bluetooth category got it to work immediately.

# Docking station

I bought a docking station for this laptop. The docking station really improves on the usability of the computer. Because this used the OneLink+ dock, it features a flexible side connector instead of the standard Lenovo laptop dock that you press a laptop into (I found those to be finicky). This makes it very convenient to plug in and out. The dock features two DisplayPorts, and six USB ports, Ethernet, a 3.5mm Audio Jack, and even a VGA output.

All of this is amazing because I have to work with USB devices a lot and having additional screens help. The number of outputs is on par with some of the modern USB-C docks today, and I'm very happy with the purchase, which set me back by $16.

# LTE (WWAN)

I noticed that my laptop did not come with WWAN which is effectively an LTE module for laptops. It didn't bother me at first because I've never used a laptop with WWAN, but I began toying with the idea of having data on the go. It would definitely be nice to have that in a place where the WiFi is slow and not have to use my phone as a hotspot. Plus in some places like a train or bus, it can get challenging to fumble with a phone while balancing a laptop.

On inspection, my laptop had a spot for a spare module. Even the antenna wires were installed: it was just waiting for the module.

{{< figure src="/images/2021/11/20211126_222923-1.jpg" alt="" caption="Slot for the LTE module" >}}

Hopping online to Ebay revealed that I could get an LTE module that was compatible with my laptop for just $24 dollars.

![](/images/2021/11/image-5.png)

What a steal! This same module would have cost $200+ back when it was new. I promptly bought it and waited for its arrival, which took around two weeks.

When it came, it was in a nondescript envelope in a static-proof bag, and all I had to do was to open the bottom of my laptop, slide the card into the M.2 slot and plug in the antennas.

Once that was done, I hopped onto Lenovo's website and downloaded the relevant drivers for the module and it was done. It took a minute to boot and when I inserted a SIM card I had on hand, I was immediately online.

{{< figure src="/images/2021/11/image-7.png" alt="" caption="Online but turned off to save power" >}}

It honestly feels like a superpower to be able to access the internet from anywhere without needing to look for a WiFi hotspot.

# QoL Upgrades

- Swap the Fn key and the Ctrl key in the BIOS
- Use SharpKeys to swap Ctrl and Alt so it works a bit more like a Mac's Cmd+C, Cmd+A ... etc. I find this is way better for my workflow
- Use AutoHotKey to swap Ctrl and Alt for *only *Ctrl+Tab

---

# Final Comments

I'm incredibly pleased with this Thinkpad. Apart from some scruffs, it is very well built and in great functional shape. For the overall price I paid (< 1/3 of a Macbook Air), I got a device with enough processing power, memory, and space to do 95% of what I need to do. It is light enough to be comparible to a Thinkpad.

I highly recommend looking at used laptops if you need a secondary device on a budget. They are often better what you can get new, and with a few tweaks they are more than capable of performing most development tasks.

---

# Fascinating Stuff

- [History of the Trackpoint](http://web.archive.org/web/20210906145549/https://www.microsoft.com/buxtoncollection/detail.aspx?id=60)
