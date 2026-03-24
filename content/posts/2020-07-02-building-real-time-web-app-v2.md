---
title: "Building a better bus arrival app"
date: 2020-07-02T15:21:00+00:00
slug: "building-real-time-web-app-v2"
draft: false
description: "Bus App V2! In this installment, we are  rewriting the app in Node and React."
cover:
  image: "/images/2020/06/busstopflowV2.jpg"
  relative: false
  hiddenInList: false
tags: ["javascript", "React", "Projects", "bus app"]
---

See the app: [https://bustime.neocities.org/](https://bustime.neocities.org/)

I previously documented by attempt to build a better bus arrival app here: [https://westsideelectronics.com/building-a-real-time-web-app/](https://westsideelectronics.com/building-a-real-time-web-app/). However there were significant limitations:

> "...limitation of this method is the upper limit on how many reads you can send to Firestore... This is not scalable.

In the case of my earlier bus stop app, there were some weaknesses:

- The main script has to live somewhere because it needed to be called once every minute.
- It relies on Google Firebase, while a great service, however, I didn't need all the scale or flexibility it offers.
- The web app isn't built to be responsive.
- You cannot set the bus stop from the client.
- Additional information about the bus type and the fullness of the bus was not included.

{{< figure src="/images/2020/05/upgradedwebapp.png" alt="" caption="Old vs New" >}}

Tldr; The new app addresses these points by having:

- A proper API that it calls periodically to update the webpage.
- No need to maintain a database. The data is now obtained in an ad-hoc manner.
- Because there is now a proper API, specific bus stops can be requested.
- The app is now properly responsive and is now a web app (go web manifests!) that can added to the home page of your phone.
- Additional details about the bus type and the fullness of the bus are now included.
- It looks way nicer!

# New Technologies

Inline Vue is now replaced with React. I admit that I am getting more fond of how React builds components, and if you structure it well you can get a pretty modular solution.

![](/images/2020/06/image-10.png)

I took what I learned from building the first app and converted the code that called the Datamall API from Python into NodeJS, which is more suited to act as an API server. On top of the NodeJS server, I also put in place an API that allows the client to query for a specific bus stop. In summary, I moved the updating logic from the server (previously the server had to update a database for the client to read), to the client (now the client queries the server at a set rate).

The main benefit of this method is that I got rid of the dependency on the Firestore database, removing the previous limitations on the read/write rates. Since the server now doesn't have to update a given bus stop constantly, a greater variety of clients can be served. Clients can also query for a specific bus stop instead of being limited to the ones that the server choses to update.

![](/images/2020/06/image-9.png)

Bootstrap is now replaced with Tailwindcss which works great for styling small components. With some CSS knowledge, the webpage is also properly responsive instead of being reliant on a framework.

Since React uses a virtual DOM (like Vue), the webpage can be updated without having to refresh the page.

The webpage has also been rewritten to present each bus stop as an element in the DOM instead of updating a HTML list.

## Weaknesses of polling

Currently I am relying on polling to obtain new data for the webpage. Previously I was relying on subscriptions to update the data on the webpage. There are some weaknesses to relying on polling. At low quantities this is a non-issue, however at high enough quantities the server might be overwhelmed with similar requests.

We know that the core API updates the data every minute. Therefore, there is no need for us to update the data more often than one minute.

One nice middle ground would be to cache the request and expire it after 1 minute. Therefore we save on polling the core API needlessly and avoids overloading our own server.

# Next Post
{{< bookmark
  url="https://westsideelectronics.com/vue-vs-react/"
  title="Rewriting the Bus App in Vue"
  description="A comparison of a VueJS and React implementation of the same web app. Visually similar, fundamentally different? Let's find out."
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
