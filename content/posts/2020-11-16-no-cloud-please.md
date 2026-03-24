---
title: "Opinion: No Cloud Please"
date: 2020-11-16T14:35:35+00:00
slug: "no-cloud-please"
draft: false
description: "Needing a cloud service to operate your Smart Home products is not always the best option, and here in this post I explore why"
cover:
  image: "/images/2020/11/server.jpg"
  relative: false
  hiddenInList: false
tags: ["opinion", "IoT", "2020", "cloud"]
---

I've come to a rule of thumb that I feel is quite useful in measuring the longevity of an IoT product: **does it require the cloud?**

> Please note that I am in **no way recommending for or against** any of the companies that are listed below as examples. They simply provide useful examples to illustrate the point.

## Requires Cloud to work, no subscription

These are the riskiest class of products. The company can turn off cloud access at any moment, rendering the product useless. Without a subscription model, it is unlikely the company will be able to keep funding their cloud services, even if it is promised to be 'lifetime' in the marketing material.

**Revolv Hub** had arguably the best IoT hub on the market, with the capability to talk to anything with 9 radios with its iconic red hub. However, after being acquired by Nest in 2014, the service was shut down in 2016, and the devices were bricked by an update, despite promises of a free lifetime cloud service. This caused a big hullabaloo that even caused the FCC to weigh in. Eventually, all Revolv Hub owners were offered a refund by 2019.

**Wink Labs **decided to charge a monthly fee in June 2020 after years of free access. Users that decide not to pay a monthly fee will lose access to updates and API services.

**Jibo **was a stationary social robot started out as an Indiegogo campaign in 2014 and ended in 2019 when the company that bought over Jibo decided to turn off the servers that gave the robot it's smarts, effectively bricking the device.

**Anki Vector** was another social robot that relied on cloud servers to do the image and voice recognition. It was almost shut down, but was bought over by another company at the last second. However, with no subscription model, it is hard to see how the cost of running the servers can be justified in the long run.

**Staples Connect **entered the market in 2013 with a hub that could connect to all and any IoT devices like the Revolv. It was later sold off to Zonoff in 2016 when the company decided to exit the market althogther. Zonoff's staff themselves were hired by Ring and that finally caused the hub to shut down in 2017.

Another type of product that falls into this category is one that requires a one-time registration before it can be used as an offline product. However, the caveat still applies. If the authentication servers are taken offline, when it comes time to reset or reregister the product, it will not work.

**Recommendation: **Avoid products that require the cloud to work. Companies have no incentive to keep the devices you buy running.

## Requires Cloud to work, subscription model

Having a subscription model addresses the question of revenue, but it still doesn't address the problem of what happens when a provider decides to shut down their cloud service. Unless the provider is part of Big Cloud: Google, Azure, or Amazon, where running a cloud is core to their expertise and revenue model, most providers may be unwilling or unable to keep their cloud services running.

This is especially true for devices that require storage or a high bandwidth like security cameras.

**Lowes Iris Smart Home** was one such service. The US home improvement store started in 2012, but even after moving to a subscription model, it decided to shut down in 2019.

Another approach that you see with this business model is that IoT devices are discounted so as to lure customers into their ecosystem and services.

**Recommendation: **Proceed with caution. Know the failure points and design in redundancies. Do not build any critical systems that require these services as your access to them can be terminated at any point.

To avoid the 'service' capture, where you are essentially held hostage to paying for cloud access for a device to continue working, consider what devices actually need to access the cloud for their basic operation. A simple light switch does not require the cloud to operate, no matter how much the salesperson wants you to believe so.

## No cloud, no subscription model

This is effectively an offline device that you can use in your own network. Lots of companies follow this model, Philips Hue, IKEA... etc. They might offer integrations to other paid services, but the devices can work just as effectively offline.

The only drawback is that companies have no incentive to keep your device up to date, and they might be open to exploits in the future.

**Best Buy **shut down it's Insignia Connect service in November 2019, but opted to let users keep using its products as dumb products. However, products like the security camera which require the cloud to stream video have essentially become paperweights.

Another class of products that fall into this category are products that offer integration with other ecosystems. The weak link here is that the integration is not guaranteed, especially if no one is paying for it. Therefore, this integration can also go away at any time.

**Google** ended its **Works with Nest** program, which was originally open for manufacturers and other services to integrate their products, to a more closed off **Works with Google.** Services that previously worked with Nest, like the popular IFTTT, lost the capability of being able to control Nest devices.

**Recommendation: **Use with caution.

## No cloud, subscription model

Ironically, this might be the best indication of a product that you can consider adopting. I hear you say: "but I'm not getting anything! Why should I pay?", but consider that the subscription to the service usually gets you additional functionality, such as firmware updates, or the ability to integrate with other products.

The subscription model also provides an incentive for the manufacturer to keep the product updated and connected to the latest services.

I actually hesitate to call this the no-cloud model, since you'll still be paying for access to a cloud. Just that this connection is not necessary for the device to operate.

Services like **IFTTT** and **Adafruit IO** follow this model.

**Recommendation**: Adopt if your use case calls for it.

---

Reliance on a cloud service for operation is not always ideal, especially if it is proprietary. While there are lots of stable cloud-based operations, the unique twist when it comes to IoT is that it is dependent on hardware sales, which is a challenging market in it's own right. Without the hardware sales to justify running the cloud, oftentimes companies just turn off the cloud altogether rather than run into the red.
