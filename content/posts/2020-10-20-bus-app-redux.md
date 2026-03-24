---
title: "Bus App, Redux"
date: 2020-10-20T07:08:52+00:00
slug: "bus-app-redux"
draft: false
description: "What came from the cloud, goes back to the cloud. Migrating the bus app from the server back to the cloud!"
cover:
  image: "/images/2020/10/gloud-1.jpg"
  relative: false
  hiddenInList: false
tags: ["bus app", "Google", "cloud", "functions", "GCP", "nodejs", "webserver", "Singapore", "arrival"]
---

I triumphantly declared that the Cloud was simply too expensive for me in my second post about the bus app, and migrated from Google Firebase to my own hosted instance to save costs.
{{< bookmark
  url="https://westsideelectronics.com/building-real-time-web-app-v2/"
  title="Building a better bus arrival app"
  description="Bus App V2! In this installment, we are rewriting the app in Node and React."
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
Yeah, about that. Turns out to be quite involved the deeper you get into it

I was* happily* revelling in my newfound power of managing a cheap server:

- Monthly fight clubs with bots attempting to overwhelm my SSH port. *I see you 124.131.2.1, and I raise you a ban for that whole IPv4 address range.*
- Trying to remember how I set things up. *Wait how did I set up https again?*
- Updating and maintaining the server. * Look at me. I'm the sudo now.*

Unfortunately, the last straw came when my preferred cloud provider sent me a nice email informing me that the prices would be raised. Again. For a second time this year. It is not a huge amount, working out to roughly the price of a coffee a month$^1$, but that represents a nearly 50% increase from my original bill.

{{< figure src="/images/2020/10/image-11.png" alt="" caption="Nice email from my cloud provider" >}}

Yes Scaleway, why? Digital Ocean and Linode both continue to offer per month pricing at reasonable rates: it was \$5/month three years ago, and it is still \$5/month now. Unfortunately, while Scaleway had the best price + performance on the market when I started out (~\$3/month), the prices have been continually rising each year.

To me, it isn't clear how monthly capping prevents Scaleway from implementing spot pricing and reserved instances. After all, it is really on how you manage billings. I would also challenge that monthly capping is not relevant. Big Cloud$^2$ allows you to rent instances if you are planning to use them continuously for a month at a lower cost per minute than if you were using the instances in an ad-hoc manner.

I think the subtext is: we're not making enough to continue providing this service, so we are raising the prices. To me that is fair enough reason. I just wish they had just put it out there.

I decided to migrate my applications off a standard server and instead focus on a whole new world of **serverless applications.** In a way I'm grateful for the push.

---

# Migration

Migrating the backend code to Google Cloud Functions turned out to be relatively painless. Because the application was written in NodeJS, and since most serverless offerings can run NodeJS, all I had to do was made some minor tweaks to adapt the code:

- Remove `app.listen` because it is now a function and there is no need to start a server.
- Set `NodeJS` runtime to `v10` in order to use `async function *` used in `got`.
- Make the main application route more explicitly `async` by adding the `async` and `await` keywords when requesting the api.
- Change `app.get` to `app.use`, converting the function to a piece of middleware.

On the frontend, I simply had to point it to the new API address and everything worked.

The great thing about serverless functions is that there is no need to manage the security aspects other than that relating to your application. Since I am using a mostly standard NodeJS runtime, I don't need any strange configuration tweaks that might require control of the server instance. Also, since my application is not called that often, it definitely has a lower cost than running a full server instance.

This was definitely less painful than I expected.

Until I wake up to a \$200 bill$^3$.

---

[1] This is how I like to count costs.

[2] Big Cloud (Like Big Oil) consists of Google, Amazon Web Services (AWS) and Azure (Microsoft). 'Big Cloud' is not a thing but I'm trying to make it a thing.

[3] No cloud provider lets you set a hard limit on your spending.

# Related posts
{{< bookmark
  url="https://westsideelectronics.com/vue-vs-react/"
  title="Rewriting the Bus App in Vue"
  description="A comparison of a VueJS and React implementation of the same web app. Visually similar, fundamentally different? Let's find out."
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}Rewriting the front end in React and Vue{{< bookmark
  url="https://westsideelectronics.com/building-real-time-web-app-v2/"
  title="Building a better bus arrival app"
  description="Bus App V2! In this installment, we are rewriting the app in Node and React."
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}Using my own server{{< bookmark
  url="https://westsideelectronics.com/building-a-real-time-web-app/"
  title="Building a real-time web app with Google Firebase, Python, & Vue"
  description="JSBuilding a real time web app to track the arrival time of buses with Python, Firestore, and VueJS!"
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}The OG app
# References

- [https://stackoverflow.com/questions/15601703/difference-between-app-use-and-app-get-in-express-js](https://stackoverflow.com/questions/15601703/difference-between-app-use-and-app-get-in-express-js)
- [https://stackoverflow.com/questions/60646437/syntaxerror-unexpected-token-when-use-gotnpm-in-firebase-functions](https://stackoverflow.com/questions/60646437/syntaxerror-unexpected-token-when-use-gotnpm-in-firebase-functions)
