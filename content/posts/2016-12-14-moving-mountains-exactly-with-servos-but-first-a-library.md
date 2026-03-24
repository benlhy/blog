---
title: "Moving mountains, exactly with Servos, but first, a Library (13)"
date: 2016-12-14T17:21:47+00:00
slug: "moving-mountains-exactly-with-servos-but-first-a-library"
draft: false
tag: ["servo", "motor", "ULCEK Tutorials", "libraries", "learning", "::", "functions", "classes", "Guides"]
---

This works on the **WeMos Mini**. It should work on the **Digispark**, however, the servo is sporadic, indicating that there might be issues with the ability of the timer to keep up.

Servos are extremely useful for positioning. They also don't draw that much power and don't require external chips to run, so they're a good way of getting an introduction to motors.

Servos have three pins and they correspond to power, ground, and data. We will be using a micro servo known as the SR92, which goes a 180 degrees. It doesn't have a great degree of precision, but it is sufficient for our purposes.

A servo works by reading the requested angle on the data pin, and checking if it is at the right position, if it is, it does nothing, otherwise, it turns. You will have to continuously update the servo because it checks at regular intervals.

Very fortunately for us, there is already a library developed by Adafruit that we can use. But copying and pasting defeats the purpose of learning, so coupled with this tutorial is a tutorial on how to understand code! This is useful if you run into any issues in your code or are trying to understand how a new library works.

Now head on to [Adafruit's Repo](https://github.com/adafruit/Adafruit_SoftServo) and add the library as a zip file to your Arduino installation. Now open the downloaded zip file and open the `Adafruit_SoftServo.cpp` file. This is where the body of the library is.

Just a quick refresher, a function encapsulates your code to make it easier to read and implement. Think of it as a machine where you put something in, the function processes it, and something else comes out. It helps to keep code abstract and easier to follow. A class/object is a way for us to define things.

For example, an `apple` is a `fruit` class. With the apple, the associated functions are `eat`, `juggle`, and `throw`. On the other hand, a `horse` is an `animal` class. It's associated functions are `ride`, and `tame`. It would be nonsensical to `tame` an `apple`, or `juggle` a horse. So is the same with objects and their associated functions in Arduino.

In the file you'll see what looks like a bunch of functions, except that they have this strange `::` that we've never seen before. Well, what they do is that they tell the compiler, hey, this function belongs to this class. So for example:

```
void Adafruit_SoftServo::attach(uint8_t pin)

```

In this case `attach` is a function that belongs to the `Adafruit_SoftServo` object. Notice that `void` appears in the front, which means that this object doesn't return any value. In the brackets, the attach function requires a variable that it refers to as `pin` and this has to be of the `uint8_t` type, which is an unsigned(non negative) 8 bit integer, which means it can go from 0 to 28-1 (0 - 255). The name of the variable is only used within this function, and when you give a value to this function, it doesn't have to be named 'pin'.

Now let us examine the example:

```
#include <Adafruit_SoftServo.h>  // SoftwareServo (works on non PWM pins)

// We demonstrate two servos!
#define SERVO1PIN 0   // Servo control line (orange) on Trinket Pin #0
...
Adafruit_SoftServo myServo1, myServo2;  //create TWO servo objects

void setup() {
  ...
  myServo1.attach(SERVO1PIN);   // Attach the servo to pin 0 on Trinket
  ...
}

```

Now we have to `#include` the library if we want to use the object types. We see that we have defined the variable `myServo1` to be the `Adafruit_SoftServo` object. In `setup` we call the function `attach` from `myServo1` and we pass it the `SERVO1PIN` variable, which happens to be 0.

So we race back to the library and look deeper into `attach`:

```
  servoPin = pin;
  angle = 90;
  isAttached = true;
  pinMode(servoPin, OUTPUT);

```

So now we have passed 0 to `servoPin`, which is then used to declare 0 as an `OUTPUT`. This object also now sets the `angle` variable as 90, and `isAttached` to `true`. These values will be used in other functions within the library.

Now you've learnt to read and use a library! Congratulations.

So now you can use the other functions within the library. We must first `attach` the pin to the servo, and then we can start using `write` to turn the servo. A basic test would be to rotate it.

```
  if (direction == 1 && angle<180){
    angle = angle + 1;
    myServo1.write(angle);
  }
  else if (direction == -1 && angle>0){
    angle = angle - 1;
    myServo1.write(angle);
  }
  else {
    direction = -1 * direction;
  }
  delay(15);

```

We rotate the servo one way until it reaches a certain angle and then we flip the direction and rotate it until it reaches < 0, and we will do this in a loop so that the servo rotates back and forth with a delay of 15 milliseconds.

#### Sketch for Digispark

```
#include <Adafruit_SoftServo.h>  

// We demonstrate two servos!
#define SERVO1PIN 0   // Servo control line (orange) on Trinket Pin #0

Adafruit_SoftServo myServo1;

int direction = 1;
int angle = 0;
   
void setup() {
  // Set up the interrupt that will refresh the servo for us automagically
  OCR0A = 0xAF;            // any number is OK
  TIMSK |= _BV(OCIE0A);    // Turn on the compare interrupt (below!)
  
  myServo1.attach(SERVO1PIN);   // Attach the servo to pin 0
  myServo1.write(90);           // Tell servo to go to position per quirk
  delay(15);                    // Wait 15ms for the servo to reach the position
}

void loop()  {
  if (direction == 1 && angle<180){
    angle = angle + 1;
    myServo1.write(angle);
  }
  else if (direction == -1 && angle>0){
    angle = angle - 1;
    myServo1.write(angle);
  }
  else {
    direction = -1 * direction;
  }
  delay(15);
}

// We'll take advantage of the built in millis() timer that goes off
// to keep track of time, and refresh the servo every 20 milliseconds
volatile uint8_t counter = 0;
SIGNAL(TIMER0_COMPA_vect) {
  // this gets called every 2 milliseconds
  counter += 2;
  // every 20 milliseconds, refresh the servos!
  if (counter >= 20) {
    counter = 0;
    myServo1.refresh();
  }
}

```
