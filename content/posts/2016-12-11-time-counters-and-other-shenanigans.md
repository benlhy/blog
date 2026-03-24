---
title: "Time, counters, and other shenanigans (11)"
date: 2016-12-11T15:51:10+00:00
slug: "time-counters-and-other-shenanigans"
draft: false
tags: ["time", "DS1307", "RTC", "ULCEK Tutorials", "Guides"]
---

Microcontrollers are not really good at keeping time, no really. Over long periods of days or weeks of continuous running, they start to lose seconds and minutes. That's why we have Real Time Counters (RTC) to help us keep time. They are microcontrollers as well, but their sole purpose is to help your main microcontroller keep time so that it can aspire to greater things, like an alarm clock or an automated cat-feeder.

One of the more popular RTC modules on the market is the DS1307. This comes as an 8 pin chip and assembled modules can be found online for cheap, or if you fancy it, you can also assemble a module from [Adafruit](https://www.adafruit.com/product/264).

Luckily for us, a library has already been written for the DS1307 by Jeelab and forked by Adafruit and can be found on [github](https://github.com/adafruit/RTClib). The DS1307 is run on I2C, and it's SDA and SCL pins should be connected to the respective pins on the modules you are using. For the Digispark it is D0 and D2 whereas for the WeMos chip it is D2 and D1 respectively. As a reminder you can consult the chip capabilities page for a quick summary of the pin functions.

In our code, we must include the RTC and Wire libraries like so:

```
#include <Wire.h>
#include "RTClib.h"

```

Next we define an RTC object:

```
RTC_DS1307 rtc 

```

We want to test our RTC module, so we set our Serial, and test if our RTC is running under `setup`:

```
Serial.begin(57600);

if (! rtc.isrunning()) {
  Serial.println("RTC is NOT running!");
}

```

We also set a time for the RTC based on the time the program was compiled:

```
rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));

```

Next in the `loop` we can retrieve the time by creating a different object called `DateTime` which is specified in `RTClib.h`. This object has several functions that allow us to retrieve the time such as `hour` and `day`. These functions take no arguments and return the value that is specified in the `now` object:

```
DateTime now = rtc.now();
Serial.print(now.year(), DEC);
Serial.print('/');
Serial.print(now.month(), DEC);
Serial.print('/');
Serial.print(now.day(), DEC);
Serial.print(now.hour(), DEC);
Serial.print(':');
Serial.print(now.minute(), DEC);
Serial.print(':');
Serial.print(now.second(), DEC);
Serial.println();

```

Notice that there is a second parameter in print. This specifies the base of the value being printed. `DEC` is decimal (base 10), `HEX` (base 16), and `BIN` (base 2). The number of decimal points can also be specified instead of the base in this parameter.

Optionally you can also calculate a time into the future by using the `TimeSpan` function. We create another `DateTime` object give it the value of the sum our `now` object and `TimeSpan`, which is the same type of object.

If the objects were not the same the compiler would complain that the objects are not of similar type. For instance you can try adding a string and a number together. In this case the computer doesn't know whether to treat this new object as a string or a number, and this is important because the operations that apply to each object is different. So always make sure you are working with the same type of object.

But coming back to the present, `TimeSpan` takes 4 arguments, days, hours, minutes, seconds.

```
DateTime future (now + TimeSpan(7,12,30,6));

```

So that's really all there is to it. Now that you have added a long term time-keeping functionality to your module, it can be used in all sorts of timing functions. You can request it to take data every two hours of the temperature, or use it to display the time.

#### FAQ

- **Help my RTC module is not displaying nonsense!**
Did you switch around the SDA and SCL pins?

- **Help my RTC module is not running!**
Did you check that 5V is being supplied to the 5V pin and you have a battery in? Does the battery have good contacts and does it provide it's specified voltage?

- **Help my RTC displays the right time but is not increasing!**
The solder connections might not be good. Check your solder connections.

- **Help I checked everything and nothing works!**
You might have a faulty crystal or module. You might want to switch out this RTC module for another one and see if the problem persists.
