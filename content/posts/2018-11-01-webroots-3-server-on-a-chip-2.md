---
title: "Webroots 3: Server On a Chip"
date: 2018-11-01T13:57:00+00:00
slug: "webroots-3-server-on-a-chip-2"
draft: false
---

I previously hinted that the ESP8266 was capable of much more and that includes being able to run a server. It is able to turn itself into a tiny WiFi hotspot and serve a basic webpage.

This avoids the hassle of looking for an access point for the ESP8266 if you just want to connect directly to it to toggle some pins.

We will require a library:

```
#include <ESP8266WiFi.h>
#include <Adafruit_NeoPixel.h>

```

We want to be able to read data as well as run our NeoPixel ring. We will also have to create a server by creating a new object.

```
#define INPUT_PIN A0
#define NEOPIXEL_PIN D6
#define NUMPIXELS 12

const char* password = "password"; // password for your network
const char* networkname = "ESP8266 Network"; // password for your network

WiFiServer server(80); //It takes in a port, standard is 80

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, NEOPIXEL_PIN, NEO_GRB + NEO_KHZ800);

```

In `setup`, we will define a few new functions so that it doesn't clutter our code:

```
void setup() 
{
  pinMode(INPUT_PIN,INPUT);
  pixels.begin();
  pixels.clear();
  setupWiFi();
  Serial.begin(9600);
  server.begin();
}

```

Below `loop`, we will write the new function, `setupWiFi()`:

```
void setupWiFi() {
}

```

It starts as `void` because there is no return value. It just configures the access point internally. We have to set the WiFi to Access Point mode, and give it a name.

```
void setupWiFi() {
  WiFi.mode(WIFI_AP);
  WiFi.softAP(networkname,password);
}

```

Now you might be wondering where did the object `WiFi` come from as we did not define it. The answer lies in `#include <ESP8266WiFi.h>`, and can be seen here: [ESP8266WiFi.h](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/ESP8266WiFi.h) and at the end of that file is the line `extern ESP8266WiFiClass WiFi;`, which declares `WiFi` to be of a type `ESP8266WiFiClass` which we can use when we invoke this `.h` file.

The next part is to code the webpage. Unfortunately, the webpage is held in a `String` so for the ease of programming, we will code the webpage with proper indentation.

```
HTTP/1.1 200 OK
Content-Type:text/html
<!DOCTYPE HTML>
<html>
<h1>Main Page</h1>
<p>Neopixel Ring: <a href="NeoPixelRingOn"><button>ON</button></a>
<a href="NeoPixelRingOff"><button>OFF</button></a></p>
<p>Analog Read: </p>
</html>

```

Now we have to convert this into a single line. The problem with this is that we have to code the newlines in (\r\n) and make sure we don't accidentally include the quotes. The lines should also be combined.

```
String s = "HTTP/1.1 200 OK\r\nContent-Type:text/html\r\n\r\n<!DOCTYPE HTML>\r\n<html>\r\n<h1>Main Page</h1>\r\n<p>Neopixel Ring: <a href=\"NeoPixelRingOn\"><button>ON</button></a>\r\n<a href="NeoPixelRingOff"><button>OFF</button></a></p>\r\n";

```

We have to set up the code for the server in loop to handle requests when clients connect.

```
void loop () {
  WiFiClient client = server.available();
  if(!client) {
    return;
  }
  String req = client.readStringUntil('\r');
  Serial.println(req);
  client.flush();
  if (req.indexOf("NeoPixelRingOn") != -1)
    turn_on();
  else if (req.indexOf("NeoPixelRingOff") != -1)
    turn_off();

  String s = "HTTP/1.1 200 OK\r\nContent-Type:text/html\r\n\r\n<!DOCTYPE HTML>\r\n<html>\r\n<h1>Main Page</h1>\r\n<p>Neopixel Ring: <a href=\"NeoPixelRingOn\"><button>ON</button></a>\r\n<a href="NeoPixelRingOff"><button>OFF</button></a></p>\r\n";
    
  s += "<p>Analog Read: </p>";
  s += String(analogRead(INPUT_PIN));
  s += "</html>\n";
  client.flush();
  client.print(s);
  delay(1);
}

```
