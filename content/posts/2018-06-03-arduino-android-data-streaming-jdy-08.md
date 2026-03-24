---
title: "How to stream data from Arduino to Android with the JDY-08"
date: 2018-06-03T12:52:07+00:00
slug: "arduino-android-data-streaming-jdy-08"
draft: false
description: "How to get the JDY-08 module to stream data to your Android phone!"
cover:
  image: "/images/2018/06/jyd08.jpg"
  relative: false
  hiddenInList: false
tag: ["Arduino", "Android", "JDY-08", "BLE", "transparent", "mode", "HM-11", "Projects"]
---

The JDY-08 is a tiny Bluetooth module that is really cheap and comes with everything on board. It is a easy and quick addition to existing Arduino projects to give them a little wireless magic. It is better than the HM-11/HC-05 boards because it uses BLE, which has lower energy consumption, takes up less space, and is still able to stream data transparently.

This is a description of how to set up the JDY-08 to stream data from Arduino to Android without installing alternative firmware. It uses the default configuration that ships with the module.

---

I will describe how to use the JDY-08 module to work in Serial transparent mode with Arduino and Android. This means that you can stream data from the MCU directly into your Android phone. This post continues on from my [JDY-08 breakout board post](https://westsideelectronics.com/building-a-hm-11-bluetooth-board/).
{{< bookmark
  url="https://westsideelectronics.com/building-a-hm-11-bluetooth-board/"
  title="Building an Arduino compatible HM-11 Bluetooth board"
  description="After messing around with the nRF52 for a little while, one day I wondered ifthere was a cheaper option for bluetooth modules, because all I really wantedwas the Bluetooth from the nRF52. It took awhile to look, but I was notdisappointed. I found the CC2541. IntroductionHM-11 modules have bee…"
  favicon="https://westsideelectronics.com/favicon.ico"
  site="West Side Electronics"
  author="Benjamen Lim"
>}}
If you'd like to make the breakout board yourself:
[JDY-08 breakout - EasyEDA<p>JDY-08 board with programming DC, DD pins broken out. Also, system Key and system LED pins and one GPIO pin are broken out</p>EasyEDA svg-batterysvg-battery-wifisvg-bookssvg-moresvg-pastesvg-pencilsvg-plantsvg-rulersvg-sharesvg-usereasyEDAlogosvg-logo-cnsvg-double-arrow![](https://easyeda.com/images/easyeda-thumbnail.png?id&#x3D;d5ed1fe5930602975df1)](https://easyeda.com/roastedneutrons/test)
# Getting it to work

Getting this device to work was not an easy feat. The JDY datasheet provides an okay reference of how the device works if you can parse what it is trying to say.
{{< bookmark
  url="https://docs.google.com/document/d/14mHWT3GhELCj-6yxsam0k5bjzazq8nnPoz4B_gYh04k/edit"
  title="JDY-08 Bluetooth LE Module Datasheet"
  description="TAG: JDY-08 Bluetooth LE BLE HM-10 HM-11 AT-09 CC41-A Original Reference (Chinese) : http://pan.baidu.com/s/1jIdeMDw http://www.cnledw.com/inter/upload/2016072916504828280.pdf https://pan.baidu.com/s/1nvAnmeX…"
  favicon="https://ssl.gstatic.com/docs/documents/images/kix-favicon7.ico"
  site="Google Docs"
>}}
It is very important that you **only use 3.3V** to power the module otherwise it will not work or might even be damaged. The logic levels are 3.3V, so a standard Arduino powered by 5V will not work. You will need to use an Arduino that is operating at 3.3V (a Seeeduino v3), or you can also use a Teensy. I strongly recommend using the Teensy as you will not need to fiddle around with setting the baud rate.

# Teensy

With a Teensy, you can use hardware serial which are broken out on other pins, which means that it can comfortably match the transmission rate on the JDY-08, the default being 115200 baud.

# FTDI Serial

You can also talk directly to this IC using a 3.3V FTDI Serial IC. First you are going to want to run `AT+MAC` to make sure that you are properly connected.

- If you don't get back a message, try checking that you are set to 115200 baud (default) and that you have connected the RX and TX wires correctly and the PWRC (P00) is grounded.

{{< figure src="/images/2021/07/image-15.png" alt="" caption="I use Termite" >}}

2. If you get `+ERR`, check that you are not appending anything to the end of the message.

3. To make it simpler later on, run `AT+BOUD4`. It will set the baud rate of the device to 9600 baud, which we can then use SoftwareSerial to talk to the device.

4. `AT+HOSTEN` should return `AT+HOSTEN:0` which means that it is set to slave transparent mode. If it is not set, set it with `AT+HOSTEN0`.

5. `AT+GETSTAT` should return `+STS:01`

Bit
Description

4(MSB)
0: Passthrough mode

3
1: connected, 0: disconnected

2
1: broadcasting, 0: not broadcasting

1
1: connect with password enabled, 0: connect with password disabled

6. Set the name of the device with `AT+NAME[your name here]`, I set mine as `AT+NAMEJDY-08`.

7. Once complete, disconnect PWRC from ground so that the device is no longer accepting AT commands. It is now a transparent module that pipes data from the UART to and from the device.

8. Using nRF Connect, the service we are looking for is 0xFFE0 and the characteristic is 0xFFE1. This is a TX-RX characteristic that allows you to send and receive data. Try typing a few characters in the terminal and sending them to the device. You should see the same characters reflected in the app.

# Arduino

Setting up the module is a little complicated in Arduino because Arduino has only one Serial port that is used to program the board.

I am using a Arduino Pro Mini that is clocked to 3.3V and 8Mhz. You can use any 3.3V boards.

Firstly, because the default baud rate on the Bluetooth module is too high for `SoftwareSerial` to work reliably, I lowered it from 115200 baud rate to 9600.

The only way to* *communicate with the Bluetooth module in order to lower the baud rate is to use RX0 and TX1 on the Arduino, which also happens to be the programming port of the Arduino. If this is a little confusing, it is because it is. We will be telling the Arduino to print a series of commands over `Serial`, which if we connect it to the module, will change some settings on the module.

While doing AT commands, be sure to **ground the PWRC (P00) line** and **turn off any Bluetooth connections** as recommended in the datasheet. Note that you should program the Arduino first before connect it to the module. Upload the following code to the Arduino you are using:

```C
#include <SoftwareSerial.h>

// Swap RX/TX connections on bluetooth chip
//   Pin 3 --> Bluetooth TX
//   Pin 2 --> Bluetooth RX
SoftwareSerial mySerial(2, 3); // RX, TX

void setup()
{
  Serial.begin(115200); // sets up Serial to reprogram the Bluetooth Module
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  Serial.println("Init complete!");
  mySerial.begin(9600);
  Serial.write("AT+BOUD4"); // set to 9600 
   
  mySerial.write("AT+BOUD"); // make sure it is set to 9600
  delay(1000);
  while (mySerial.available()) {
      Serial.write(mySerial.read()); // it should return +BOUD:4 if the baud rate setting was successful
   }
}
void loop() {}

```

With this code, connect pin 0 (RX) and pin 1 (TX) on the Arduino to the JDY-08 module's TX and RX pins. Ground the PWRC (P00) pin and reset the Arduino. The main Serial port should have written `AT+BOUD4` to the JDY-08. Unfortunately, there is no way to check if we were successful or not at this point.

Once this code is uploaded, we can switch the TX and RX lines to pins 2 and 3 on the Arduino, which will use `SoftwareSerial` to communicate with the Bluetooth board. To check that the reprogramming was successful, reset the board once you have reconnected the wires. Software Serial should now be able to talk to the Bluetooth board and query the baud rate, and you should see `+BOUD4` printed to the Serial Terminal (set to 115200).

Now since we just want the Bluetooth module to transparently transmit data to an Android device, so we will use the APP mode of the device. Here the following Arduino code uses `sprintf` to create a string with a variable that updates every one second. Note that you have to **remove P00** once all AT commands have been completed.

```C
#include <SoftwareSerial.h>

// Swap RX/TX connections on bluetooth chip
//   Pin 2 --> Bluetooth TX (P03)
//   Pin 3 --> Bluetooth RX (P02)
SoftwareSerial mySerial(2, 3); // RX, TX

void setup()
{
  Serial.begin(115200);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  Serial.println("Init complete!");
  mySerial.begin(9600);

  Serial.write("AT+BOUD4");
   
  mySerial.write("AT+BOUD4");
  delay(1000);
  while (mySerial.available()) {
      Serial.write(mySerial.read());
   }
     Serial.write("AT+HOSTEN0");
  mySerial.write("AT+HOSTEN0");
  delay(1000);
  while (mySerial.available()) {
      Serial.write(mySerial.read());
   }
  Serial.write("AT+GETSTAT");
  mySerial.write("AT+GETSTAT");
  delay(1000);
  while (mySerial.available()) {
      Serial.write(mySerial.read());
   }

  Serial.println("Complete!");
  delay(10000); // delay, pull out or otherwise disconnect P00 from ground
}

int i = 0;
char mybuffer[50];
void loop() {

  sprintf(mybuffer,"This number is: %d",i);
  mySerial.write(mybuffer);
  Serial.println(mybuffer);
  i++;
  delay(1000);
  if (Serial.available()) {
    mySerial.write(Serial.read());
  }
}

```

## Android

On your Android phone, download the excellent **nRF Connect for Mobile** by Nordic Semiconductor and scan for a device named JDY-08. Once connected, open the first unknown service and the first unknown characteristic. Under *value*, you should see a string saying *The number is: xxx*, and it updates over time.

Now you can stream data from Arduino to your phone. With a little Android magic, you can then set values and even plot the live data on your phone from your Arduino.

## Conclusion

Getting here has taken quite a bit of time and I documented as much of the process as possible. This would not have been possible without those who found the datasheet and translated it into English. The next steps are to create an app that reads this value and processes it into something useful.
