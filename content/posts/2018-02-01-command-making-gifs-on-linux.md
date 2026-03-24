---
title: "Command for making GIFs on Linux"
date: 2018-02-01T15:12:00+00:00
slug: "command-making-gifs-on-linux"
draft: false
cover:
  image: "/images/2018/01/haha.jpg"
  relative: false
  hiddenInList: false
tag: ["linux", "Ubuntu", "gifs", "command", "line", "GIF"]
---

Making GIFs on a Linux computer requires you to jump through a few hoops, after a few hours of experimentation, this is my solution. Of course, you can also use an online service.

`ffmpeg -ss [start] -i input.mp4 -t [duration] -vf scale=280:-1 -r 15 -f image2pipe -vcodec ppm - |   convert -delay 5 -loop 0 - output.gif`

The time should be set in the following format: `hh:mm:ss` for example, `00:12:31`.

![output-1](/images/2018/01/output-1.gif)
