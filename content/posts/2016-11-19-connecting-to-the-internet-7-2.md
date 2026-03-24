---
title: "Uploading without wires (7)"
date: 2016-11-19T05:12:31+00:00
slug: "connecting-to-the-internet-7-2"
draft: false
cover:
  image: "/images/2016/11/ota-7.jpg"
  relative: false
  hiddenInList: false
tag: ["esp8266", "ULCEK Tutorials", "ota", "wifi", "Guides"]
---

This tutorial is only for the ESP8266 chip.

If you have a ESP8266 chip, we have merely used 5% of this chip's total capabilities. The ESP8266 chip can both connect to Access Points (AP) and also set up a soft AP, which cleans that it can serve as both a client and router. On this router we can serve up webpages to control what we want the chip to do. One very useful method to interface with your chip over wifi is Over The Air (OTA) updates.

What this means that you connect to a router, and it awaits instructions over the local network. This allows you to reprogram your chip over WiFi, which makes programming really easy.

Now we will need to include a couple of libraries for our OTA to work:

```
#include <ESP8266WiFi.h>
#include <ESP8266mDNS.h>
#include <WiFiUdp.h>
#include <ArduinoOTA.h>

```

Now we want to give our chip the SSID (name) of the network and the password:

```
const char* ssid = "MyNetwork";
const char* password = "StrongPassword";

```

In `setup`, we want to perform some startup procedures on our chip:

```
 WiFi.mode(WIFI_STA); //we are going to be a client! 
 WiFi.mode(ssid,password); //Start the wifi, with this ssid and password

```

In order to ensure that some boot process didn't mess up our wifi chip, we are going to restart the WiFi chip if we didn't connect after awhile:

```
while (WiFi.waitForConnectResult() != WL_CONNECTED) 
{
    Serial.println("Connection Failed. Rebooting ... ");
    delay(5000);
    ESP.restart();
}

```

Now we need some indicators to tell us if we are done and/or we faced errors. These functions that we call from `ArduinoOTA` tell us if we have started, ended, and what our progress is:

```
ArduinoOTA.onStart([]() {
  Serial.println("Start");
});
ArduinoOTA.onEnd([]() {
  Serial.println("\nEnd");
});
ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {
  Serial.printf("Progress: %u%%\r", (progress / (total / 100)));
});

```

The following code helps us to diagnose the error depending on what is . Usually this process is quite reliable and I some of this code to keep the sketch shorter, however if this is your first time, having this code is certainly very useful.

```
ArduinoOTA.onError([](ota_error_t error) {
  Serial.printf("Error[%u]: ", error);
  if (error == OTA_AUTH_ERROR) Serial.println("Auth Failed");
  else if (error == OTA_BEGIN_ERROR) Serial.println("Begin Failed");
  else if (error == OTA_CONNECT_ERROR) Serial.println("Connect Failed");
  else if (error == OTA_RECEIVE_ERROR) Serial.println("Receive Failed");
  else if (error == OTA_END_ERROR) Serial.println("End Failed");
});

```

Now we have finally set up everything, it is time to start:

```
ArduinoOTA.begin();
Serial.println("Ready");
Serial.print("IP address: ");
Serial.println(WiFi.localIP());

```

The only thing you need to put in the `loop` is:

```
ArduinoOTA.handle();

```

So now that you have this bunch of code, you can add visual indicators that separate them from other code you don't when you are implementing it for other projects:

```
//============= Don't Touch Dis ==================
code code code
code code code
//============= End of Don't Touch Dis =============

```
