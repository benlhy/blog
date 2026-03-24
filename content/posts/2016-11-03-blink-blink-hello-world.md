---
title: "Blink, blink. Hello World! (1)"
date: 2016-11-03T22:08:11+00:00
slug: "blink-blink-hello-world"
draft: false
tags: ["ulck", "ULCEK Tutorials", "LED", "blink", "Guides"]
---

So you have an ULCK set in your hands. Now what?

Have you downloaded the [Arduino IDE](https://www.arduino.cc/en/Main/Software) (required for programming) and followed the guide for board definition installation for the [ESP8266](https://github.com/esp8266/Arduino), the driver installation for the [Arduino Nano](http://blog.codebender.cc/2015/08/28/tech-stuff-ch340g-drivers-for-windows/), or the complete installation procedure for the [Digispark](http://digistump.com/wiki/digispark/tutorials/connecting)? If not please install Arduino IDE before proceeding with the respective installation requirements of each board.

---

Following the time-honored tradition of all guides, a blinking LED will be your "Hello World!" for electronics. Let's get started!

When you first start the Arduino program, an empty window will pop up. This is where you'll be writing your program. In the Arduino IDE, this is known as a 'sketch'. Notice that there are two sets, one which is the `setup`, and the other will be the `loop`.

When it boots, the microcontroller will run all the commands in `setup` once, and then run the commands in the `loop` in sequential order until it is powered off. The setup will be where we tell/declare to the microcontroller what pins we are using and what we are using them as, and the loop will instruct the microcontroller what to do in each cycle.

So now here we'll have to declare to the microcontroller that we are using the onboard LED.

In Arduino, we use pins to send and receive data. These are the little pointy things on the Digispark or WeMos that plug into the breadboard. By sending data to these pins, we can control other electronics components.

In the following code below I will be using comments. These are parts which the program does not read and you'll see them to be grey-ed out. They are anything that follows `//` on that line. If you remove the `//` the code will be read by the Arduino IDE again. This is useful in adding comments to clarify what your code is doing as well as to test your code.

Enter the following between the braces `{}` after `setup`.

```
pinMode(1,OUTPUT); //For digispark
//pinMode(BUILTIN_LED,OUTPUT); //For the ESP8266 module

```

`pinMode` is the function that helps us to declare what the pin is going to be. But we have to give it two inputs: the pin number, and what the pin is functioning as. The inbuilt LED on the board has a reference of 1, so we are going to use 1 to refer to it. Since we are outputting a signal to this pin, we are going to use it as an `OUTPUT`. You will see Arduino highlight `OUTPUT` in orange because it is a keyword. All orange words serve a specific use and should not be used as variables.

Some variables are already defined. In some cases we can also use `BUILTIN_LED` to specify the onboard LED for the ESP8266.

> Reminder: Programming in Arduino is case-sensitive.

Remember to add `;` after you complete a command. This acts as a delimiter, much like how spaces in sentences help to form words. Without it the IDE will not be able to understand whatyouaretryingtosay.

Great! Now how shall we blink our LED? One second off, two seconds on? In the `loop` portion of our program, write the following.

```
digitalWrite(1,LOW);
delay(1000); //delay counts in miliseconds

```

That settles the off portion of our loop, can you guess the on portion?

```
digitalWrite(1,HIGH);
delay(2000); //delay counts in miliseconds 

```

Never had any doubt in you. `delay` as you might have guessed is another function that stops the microcontroller for the time which you specify in milliseconds. `digitalWrite` is another function that tells a specified pin to go either `HIGH` or `LOW`, this physically means that it switches the output of a pin to a high voltage or a low voltage.

Now click the upload button and plug in your ATTINY85 chip when the dialogue tells you to. It should upload and start to blink after a few seconds. If you're using an ESP8266 chip, plug it in first before uploading.

#### Sketch

The following sketch is for the ATTINY85:

```
void setup()
{
    pinMode(1,OUTPUT);
}
void loop()
{
    digitalWrite(1,HIGH);
    delay(2000);
    digitalWrite(1,LOW);
    delay(2000);
}

```

The following sketch is for the ESP8266

```arduino
void setup()
{
    pinMode(BUILTIN_LED,OUTPUT);
}
void loop()
{
    digitalWrite(BUILTIN_LED,HIGH);
    delay(2000);
    digitalWrite(BUILTIN_LED,LOW);
    delay(2000);
}

```

#### FAQ

- **Help! I have a Mac and it doesn't work!**
Try using a usb extension or a USB-2 port. The USB ports of the newer Macs are recessed and the data pins might not touch. Trying different USB ports might also work.

- **Help! The compiler is giving me an error!**
Did you complete braces `{}`, use `()` and all delimiters `;`?

- **Help! How do I upload code?**
For the Digispark, upload the code first, then plug it in. For the ESP8266 board, plug it in then upload your code.

#### What you learned

- How to define output pins.

- How to upload a sketch.

- How `digitalWrite` works.

- How `delay` works.

#### Additional Projects

- Can you change the period that the LED blinks?
