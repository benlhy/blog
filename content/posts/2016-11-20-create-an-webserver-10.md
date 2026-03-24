---
title: "Create a WebServer (10)"
date: 2016-11-20T04:42:37+00:00
slug: "create-an-webserver-10"
draft: false
tags: ["ULCEK Tutorials", "webserver", "esp8266", "Guides"]
---

With the ESP8266 it was hinted earlier that it can host a web page, and this guide serves as a quick breakdown of how to create a server that serves up webpage. This can also be found as a sketch under Examples. This tutorial assumes that you have some knowledge of HTML.

We start by including libraries:

```
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>
#include <ESP8266mDNS.h>

```

Two of these might be familiar to you, the other two might not be. `ESP8266WebServer.h` is a library that helps us to set up a web server on our ESP8266 module. `ESP8266mDNS.h` is a library that allows us to receive and manage DNS requests. Now we create an object for DNS, and write in the credentials for our network:

```
MDNSResponder mdns;
const char* ssid = "mySSID";
const char* password = "strongPassword";

```

Create our server object, with the port as a variable:

```
ESP8266WebServer server(80);

```

We want to create a string (!) that holds our web page:

```
String webPage = ""; 

```

So maybe we want to toggle a pin:

```
#define PIN D1

```

Now in `setup`, we want to create our webpage and setup our pins:

```
pinMode(PIN,OUTPUT);
Serial.begin(115200);
WiFi.begin(ssid,password);
webPage = webPage + "<h1>Web Server</h1><p>PIN!<a href=\"pin\"><button>ON</button></a> <a href=\"OFF\"><button>OFF</button></a></p>";
while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print("Restart ESP8266!");
}
Serial.print("IP address: ");
Serial.println(WiFi.localIP());

```

Now at this point we want to define what our pin does depending on which page the client is on:

```
server.on("/",[](){
    server.send(200, "text/html", webPage);
});
server.on("/ON", [](){
    server.send(200, "text/html", webPage);
    digitalWrite(PIN,HIGH);
    delay(1000);
});
server.on("/OFF", [](){
    server.send(200, "text/html", webPage);
    digitalWrite(PIN,LOW);
    delay(1000);
});

```

Now it is time to start the server:

```
server.begin();
Serial.println("Initialisation Complete.");

```

And we need to run the handling function continuously in `loop`:

```
server.handleClient();

```

So you have the chip connect to an SSID with the name of "mySSID". On Serial, you will see the IP address that it is assigned. On the web page you will see two buttons that switches on and off the LED.
