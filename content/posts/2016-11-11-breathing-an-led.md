---
title: "Breathing an LED (3)"
date: 2016-11-11T18:17:04+00:00
slug: "breathing-an-led"
draft: false
tag: ["ULCEK Tutorials", "LED", "programming", "breathing", "Guides"]
---

Blinking an LED is all fine and dandy, but it doesn't make for any pretty effects. A breathing LED, now that is something pretty to gaze upon.

But how can we do that? A microcontroller only knows two states, `HIGH` and `LOW`, how can we get a state in between these two values?

The trick is to switch your output so quickly that your eyes cannot detect this change. This is what happens when you switch on a light from an AC current. The light is actually blinking so rapidly that you can't see the difference between dark and lit conditions. This is what we'll rely on when changing the perceived brightness of our light.

The lit and non-lit timings are constant in overhead lights, so you don't detect a difference in brightness. However if we increase the time by which it is lit by just a little, it will seem brighter to us. By modifying the ratio when the LED is on and when it is off, also known as the duty-cycle, the LED can be made to appear dimmer or brighter. This method is known as Pulse Width Modulation (PWM).

On the microcontroller, on any pin, 0 corresponds to `LOW` and 255 corresponds to `HIGH`. Any value in-between scales accordingly. In order to build a breathing LED, all we have to do is to change some features in our programming and wire our LED differently.

#### Programming

Let's start by redefining our pin. Some pins are capable of PWM and others are not. Select the pin that is capable of PWM, sometimes indicated with a wavy symbol `~` and then define it in `setup` as follows:

```
analogPin(D1,OUTPUT);

```

If you've previously defined this pin as a `digitalPin` you'll have to change it. Digital signals are `HIGH` and `LOW` signals, or `1` and `0`. Analog signal on the other hand have a gradient of values. This stems from the historical context whereby scales were used to make measurements before they were replaced by computers which used this sort of fast-changing `HIGH` and `LOW` signals to make measurements.

Similarly, since we defined the output as analog, we can write an analog value to the pin:

```
analogWrite(D1,0);

```

However, we want to modify this over time, so we'll use a counter for the intensity of our light and a multiplier to make it go up and down. These are constants and we can define them outside of `setup`:

```
int count = 0;
int increment = 1;

```

And we want to increment it in the `loop` function:

```
count = count + increment;

```

But if we reach 255 we want to decrease it and if we are at 0 we want to increase the count:

```
if (count == 255 && increment == 1)
{
    increment = -1;
}
if (count == 0 && increment == -1)
{
    increment = 1;
}

```

Finally we want to set a delay so that it is a slow increase and decrease of the signal:

```
delay(300);

```

Now having all the pieces, can you put it together into a sketch?

#### Sketch

```
int count = 0;
int increment = 1;

void setup ()
{
    analogPin(D1,OUTPUT);
}   

void loop()
{
    if (count == 255 && increment == 1)
    {
        increment = -1;
    }
    if (count == 0 && increment == -1)
    {
        increment = 1;
    }
    analogWrite(D1,count);
    count = count + increment;
    delay(300);
}

```

Note that this is not the most efficient means by which you can program this behaviour. However it is easier to follow. If you have some programming experience and have an alternative solution, use it by all means.

#### Hardware

Reconnect the positive lead to the LED to the pin you defined in your program.

Upload your program and your LED should start breathing gently!

#### FAQ

- Help my LED is blinking not breathing!
Did you connect it to a PWM pin? Refer to the [chip capabilities](https://westsideelectronics.com/chip-capabilities/) page to determine which pin is capable of PWM output.

#### What you learned

- What is PWM and how it works.

- The difference between analog and digital.

- How to use `analogWrite`.

#### Additional Projects

- Fade three LEDs in a row such that it appears that a single light is passing through them from left to right.
