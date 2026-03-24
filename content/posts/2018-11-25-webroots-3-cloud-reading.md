---
title: "Webroots 3: Cloud Reading"
date: 2018-11-25T15:38:00+00:00
slug: "webroots-3-cloud-reading"
draft: false
---

The final part of this Webroots project is to read data from our sensors. This is useful because many IoT devices are intended as a means to collect data. By implementing this service, we will be able to tell the state of our room, even when we are not close by!

For this tutorial, we will be reading the button presses, the light levels, and the number of times our Night Light has been turned on.

### Counting button presses

We will need to set up a variable, which we will call `button_counter` in order to keep track of our button presses:

```
int button_counter = 0;

```

For every button press, we want to record it, and so increment it. Here we will add an additional line in our `buttonPress` function to do so:

```
void buttonPress() {
  state!=state;
  button_counter++;
}

```

The `++` is an **increment operator**. It adds 1 to the value that it is operating on. In programming, a lot of things are incremented, so in order to save time, the `++` operator is used.

We can now count. You will need to create a new feed, name it `Button`. However, you will have to use the Text Box block when setting it up.

In the code, we have to set up the feed, can you remember how to do it?

```
...
AdafruitIO_Feed *button = io.feed("Button");
...

setup() {
  ...
}

loop () {
  ...
}

void buttonPress() {
  state!=state;
  button_counter++;
  button->save(button_counter);
}

```

The `buttonPress` function is called as an interrupt, so it will only save the value when the button is pressed.

### Logging Light Levels

Logging the light levels will also tell us how the room is lit at any given time of day. The setup process is the same.

- Set up an Adafruit IO feed, using the line graph.

- Add a line for the corresponding feed to the sketch.

- Push the data every cycle. You might want to create a `for` loop to send data every 10 cycles so as to not flood the Adafruit servers.

Try it out and see if it works!

```
...
AdafruitIO_Feed *light_levels = io.feed("LightLevel");
...

setup() {
  ...
}

loop () {
  io.run(); 
  input_value = analogRead(INPUT_PIN);
  light_levels->save(input_value);
  if (input_value < BRIGHTNESS) {
    turn_on();
  }
  if (input_value > BRIGHTNESS) {
    turn_off();
  }
  delay(100);
}

```
