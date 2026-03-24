---
title: "React-Teave: Ranking teas according to your preferences"
date: 2020-05-24T00:55:30+00:00
slug: "react-teave"
draft: false
description: "Introducing React-Teave! A tea ranking site made in React that sorts rated teas to your preferences."
cover:
  image: "/images/2020/05/react-teave-1.JPG"
  relative: false
  hiddenInList: false
tags: ["tea", "React", "programming"]
---

TLDR; I made a website that allows you to return search results of teas that are tailored to a specific mix of your preferences. Check it out at [React-Teave](github.io/benlhy/react-teave).

---

Tea.

Can't live without it, but you can definitely live without bad tea. Some teas taste like they are made out of twigs, which I can assure you, is not a very pleasant taste at all.

Nobody:

Me: Hey! Why don't I share with you what **I** think about the tea I'm drinking?

It started out as a means to justify my ever growing trove of loose leaf teas, and still is a means to justify my growing trove. Don't judge.

I put together a simple website to house my thoughts about tea. The ranking is based entirely on my own experience and the descriptions run roughly along the lines of *"good tea, but lacks something"*. Except for Lipton Tea.

{{< figure src="/images/2020/05/image.png" alt="" caption="My Tea Journal" >}}

The site itself is a simple HTML site to house my reviews, with simple `div`s that are hidden until you click on the title. It uses about six lines of JavaScript to toggle the longer descriptions of the tea. However, the annoying thing was that each time I wanted to update the site, I had to copy and paste a whole section of HTML. It isn't even responsive to the size of the window.

{{< figure src="/images/2020/05/image-1.png" alt="" caption="Text does not reflow" >}}

But the site felt a little... static. *How* was I ranking the teas anyway? Could I compare the taste just through memory alone? How could I justify my placement of the *Jasmine Yin Hao* over the *Award Winning Golden Buds Jasmine Tea*?

---

I've always been frustrated by the lack of graularity in search options. Yes, I want items sorted by the number of stars **and** the number of reviews, *no I don't want Woosher's Magic Clean Mop (No cleanups!) with its perfect rating of five stars and solitary review of "Works"*. I want a balance of reliability, popularity, and price. And I want to control the balance according to my own preferences.

Then React came along. React is a framework that allows you to build interactive websites very quickly. Combining React, this sorting method, and my previously static tea website felt like a natural combination.

Making something like this, for instance:

{{< figure src="/images/2020/05/image-2.png" alt="" caption="BAM. New Tea Ranking Website, with pictures!" >}}

React-teave is an experiment, and an exercise. It upgrades the previous version of the site with React so that the page is now more interactive. You can now specify the importance of each criteria of the tea to you: how expensive it is, how difficult it is to brew, and how much it costs. The website will sort the reviews accordingly. For example, if you don't care about the cost nor the brewing ease, then the website recommends Paris Tea from Harney and Sons, and following very closely behind is the Golden Buds Jasmine Tea from Teavivre.

![](/images/2020/05/image-3.png)

This was a result that was completely unexpected to me because the Paris Tea wasn't even in the top 5 of my original list, and Golden Buds was at the bottom of these sorts most of the time due to its high cost.

It gave me a rather surprising result that the Cinnamon tea had the highest scores by default. I liked the tea, but it was 7th in my original rankings.

---

It was really fun to build this site in React and explore the idea of weighted rankings. I think the implementation works and does what I set out to do. I hope to see more options in sorting search results in the future.
