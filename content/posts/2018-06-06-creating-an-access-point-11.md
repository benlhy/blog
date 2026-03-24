---
title: "Creating an Access Point (11)"
date: 2018-06-06T01:53:00+00:00
slug: "creating-an-access-point-11"
draft: false
---

Now that you have a little bit of experience with using the WiFi on the ESP8266, we'll now set it up as its own access point. This makes it useful if you're running the ESP8266 as a standalone device, perhaps collecting data on the temperature or monitoring some sensor. It makes it easy to access the ESP8266, which might be placed in a hard to reach location.

First off, the library that we need:

```
#include <ESP8266WiFi.h>

```

Next we want to define some credentials, the same ones we've been using:

```
const char* ssid = "mySSID";
const char* password = "strongPassword";

```

Finally we want to tell the ESP8266 we are setting up an AP with the given credentials!

```
WiFi.mode(WIFI_AP);
WiFi.softAP(ssid,password);

```

That's all! This is quite simple and you can even connect it with the previous server example to serve up webpages when a device connects.

For now, let's just build a simple circuit that tells us if a client is connected. To do that, we'd have to check if a client is connected and if so, light an LED. You can chose to do so with an external or internal LED.

```
#include <ESP8266WiFi.h>

const char* ssid = "mySSID";
const char* password = "strongPassword";

WiFiServer server(80);

void setup()
{
  Serial.begin(115200);
  pinMode(BUILTIN_LED,OUTPUT);
  WiFi.mode(WIFI_AP);
  WiFi.softAP(ssid,password);
  digitalWrite(BUILTIN_LED,LOW);
}

void loop()
{
  WiFiClient client = server.available();

  if (!client) {
    digitalWrite(BUILTIN_LED,LOW);
    return;
  }
  else {
    digitalWrite(BUILTIN_LED,HIGH);
  }
  delay(1);
}

```

On another note, there is an interesting sketch that is on [github](https://github.com/tzapu/WiFiManager) written by user tzapu that serves up a proper WiFi connection manager for the ESP8266.
