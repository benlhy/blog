---
title: "Design Analysis - Sparkfun's Spectacle Project"
date: 2021-07-12T15:15:24+00:00
slug: "sparkfun-spectacle-project"
draft: false
description: "A design analysis of Sparkfun's soon-to-be retired Spectacle platform. What went wrong and what lessons can we learn? "
cover:
  image: "/images/2021/07/spectacle-logo-big-1.jpg"
  relative: false
  hiddenInList: false
tag: ["2021", "Sparkfun", "design", "Spectacle", "Platform"]
---

What a Spectacle!

Last week, I was reminded of this platform by way of browsing Sparkfun's surplus section, that the Spectacle Director board was going for 60% off.

![](/images/2021/07/image-11.png)

At that point in early 2017 I thought it was a neat concept to connect boards together but I never really followed up because it seemed to be a little limiting.

My interest piqued, I decided to check up on the other Spectacle boards, but there was a reason why the Director board was being offered at 60% off...

![](/images/2021/07/image-7.png)

I mean, if it doesn't come with other boards, what good is it?

I then decided to do a little digging and look into Sparkfun's Spectacle platform.

---

The Sparkfun Spectacle project came into being as a means for creators who don't want to code, or have to learn electronics to begin integrating it into their designs.

{{< figure src="/images/2021/07/image-6.png" alt="" caption="Sparkfun's Spectacle Platform" >}}

This is a copy from their Spectacle page:

> We designed the Spectacle platform for creators who want to use lights, sound and motion in their work but don’t want to start an electronics hobby.

I feel that if someone is interested enough to look into adding lights, sound, and motion into their project, they would also be sufficiently interested in learning a small bit of electronics to customize their project.

> *Spectacle is a product ecosystem centered around a simple idea: creative people shouldn’t have to learn new skills to use electronics in their projects. You’ve spent years developing the skills you use, and SparkFun wants to recognize that and help you expand your creations to include electronics without requiring you to spend years learning about electronics and programming.*

I think Sparkfun is a little overexaggerating the time it takes to learn and use a bit of electronics.

Sure, laying out a four layer board that takes care of the high speed signals properly will take you some time to learn, but blinking some lights or moving a servo? That takes about five minutes of googling and two seconds to copy-paste, 45 seconds to program the board, three to realize you haven't hooked up the servo, another three minutes of looking up how to hook up the servo and finally about 15 seconds, give or take a second, to connect the servo.

So roughly 10 minutes. It's not exactly learning, but it gets you to the same place.

But I digress.

New ways of implementing electronics is not a new concept. People have been trying to bring electronics onto all sorts of media for as long as electronics has existed. Some of the more recent examples are textile electronics such as the LilyPad by Sparkfun and Flora by Adafruit, paper art style electronics with Chibitronics, and lego-style electronics by LittleBits.

{{< figure src="/images/2021/07/image-5.png" alt="" caption="LilyPad Electronics Kit" >}}

One niche that Sparkfun's unit decided to target was the artist market. The thing that Sparkfun tried to do different with its Spectacle platform is that instead of using wires to connect the different units, they would use audio cables.

![](/images/2021/07/image-12.png)

At the point in time, one of the newest ideas in this space was to use audio jacks to program microcontrollers.

Unfortunately bunnie was a bit off here about his claim that there will always be an audio jack. Apple started to remove their audio jacks in 2016, and quite a few companies followed after. Today you'll find it challenging to work with Sparkfun's Spectacle board if you don't own a device with an audio jack.

This was the time too that Makerfaire and the Maker movement started to get really big and there was demand for prototyping boards that can be brought to demos that aren't just a ratsnest of wires.

With this background, I think it formed a large part of the impetus for Sparkfun's Spectacle board.

# The Good Parts

The Spectacle ecosystem when it was released, covered a good portion of input and output devices. It had boards for buttons, servos, lights and even audio.

It could be programmed directly from a phone via the phone's audio jack, eliminating the need to use a laptop to program the device. Audio jacks are quite robust and they don't pull out easily even when the boards are shifted, which makes it a huge plus in a setting where there might be a lot of movement such as a Makerfaire or a convention.

So why did it not succeed?

# Market Fit

Superficially, Sparkfun seemed to target the artist-maker market. The boards were black to be easily concealable, and they were easy to connect to each other with reliable audio cables. However, by most reports, the programming interface was confusing to use, and the boards didn't communicate well with devices.

If Sparkfun wanted this to grow the Spectacle platform for everyone to use, they could also have added in hackability or compatibility with current frameworks. However, since it was designed to be radically different, there wasn't much in the way of adding more custom DIY boards.

I can't imagine that the market for artist-makers who don't want to learn programming is huge too. Given that you are already investing some time into adding custom electronics into your project, it wouldn't have hurt to learn a little more to ensure that your project had some extensibility.

Looking at the boards, they would also be quite heavy and were probably not meant for fabric, which is a huge portion of the alternative platforms for electronics.

# Limited Customizability

While Sparkfun included lots of nifty features into the platform, like many low-code or no-code platforms, the moment you want something a little more complex the platform suddenly explodes in complexity.

For example, there is no way to, for example, add a delay between effects, which is something you'd conceivably want when installing a piece.

Something that in Arduino would have been `delay(1000);`

If all you wanted was a continuous effect, you could do the same with one of the hundreds of light controllers on Aliexpress:

![](/images/2021/07/image-8.png)

# It isn't that simple

The program's flow isn't clear at all. If you take a look at what a program looks like in Spectacle it isn't clear how the program flows. Is it from top to down? Do all of it start at once? Is it from bottom up?

{{< figure src="/images/2021/07/image-9.png" alt="" caption="Can you predict what will happen?" >}}

Can you predict what will happen? I almost guarantee that you cannot because some of the inputs, such as interval timings, are hidden in the options. I can't really understand this particular design choice, given the huge amount of space that is available on the page. Even on a mobile phone, you can just have each block take up more vertical space.

{{< figure src="/images/2021/07/image-13.png" alt="" caption="The information isn't that dense anyway" >}}

In contrast, see MakeCode's implementation:

![](/images/2021/07/image-10.png)

To me, MakeCode's is far clearer way to represent the program and the execution flow. I don't think MakeCode's implementation is unreasonably difficult that a dedicated person can learn it in an afternoon. All the information is also represented in the blocks too without additional screens to navigate to.

The programming process for the Spectacle board is also not easy.

This is the complete instruction provided by Sparkfun to install your script on the Spectacle board once you have written(?) it:

> Supply power to the Director Board via the micro USB jack on the end of the board, then hold down the RST button. Hold down the PROG button, and release the RST button. After a moment, you should see the LED on the board blinking. It should blink three times, pause, blink three times, pause, repeatedly. Turn your system's volume up all the way, then touch or click the "Install Script" button at the bottom of the screen. That will pull up the page below. Click or touch the "Install" button at the bottom. The button will gray out during the installation process. When it has returned to its normal color, the installation is done. If the installation was successful, you should see the LED on the Director Board blink 10 times, then pause, then 10 times, then pause, etc. Press the RST button on the Director. Again, you'll see 10 blinks, then a pause on the Director LED. That means the program is loaded and everything is working.

Breaking it down:

- Power the board with the USB
- Connect it to the audio jack
- Ensure the board is in programming mode
- Click a button to install the script
- Unset the board from programming mode.

For Arduino:

- Plug in the board.
- Select the connected board
- Click program.

That's two more steps than Arduino, and that's not counting having to remember what each blinking sequence means (*is 10 blinks okay or was it three?).*

The complicated steps to put the board in programming mode also doesn't help when your creation stops working in the middle of a presentation and you have to reprogram it under pressure.

# Conclusion

The lessons to be learned here is focus. Market fit, simplicity, customizability all tie back to the type of user that Sparkfun appears to be targeting. If you are building for someone who wants something that works without any programming, make sure that your entire process supports that user story. If any part of it doesn't, you just lost a portion of your audience. Given how small the audience that Spectacle was addressing, their focus had to be laser sharp for a product like that to work.

One example would have been small modular components that physically connect together, like Chibitronics, but on a grander scale.

Ultimately, the limitations of the Spectacle platform restricted its market reach and probably contributed to the end of this product line that we see today.

---

Another ecosystem that Sparkfun has come up with is the MicroMod system that uses M.2 holders that allows you to hot-swap different microcontrollers on base boards. I think it is a really exciting concept, however, the requirements are tight: 4 layer board, 0.8mm thickness, small footprint. This makes it challenging for the community to iterate on and only time will tell if it will gain widespread adoption.

#
