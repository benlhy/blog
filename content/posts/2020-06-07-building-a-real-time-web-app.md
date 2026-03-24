---
title: "Building a real-time web app with Google Firebase, Python, & VueJS"
date: 2020-06-07T09:05:43+00:00
slug: "building-a-real-time-web-app"
draft: false
description: "Building a real time web app to track the arrival time of buses with Python, Firestore, and VueJS!"
cover:
  image: "/images/2020/06/busstopflow.JPG"
  relative: false
  hiddenInList: false
tags: ["Python", "Firebase", "GCP", "Firestore", "VueJS", "frontend", "backend", "javascript", "bus app"]
---

This is my first attempt in building a real-time web app that helps to display bus arrival times at a bus stop so that I can catch buses without waiting.

Tldr; I built an app that displays the buses in order of arrival with the time that it will take for each bus to arrive. Check it out at [timetable.neocities.org](https://timetable.neocities.org).

{{< figure src="/images/2020/06/image-8.png" alt="" caption="Buses in order of arrival" >}}

---

At my bus stop I want to know which buses are arriving soon because there are a few specific bus services that stop at a more convenient location. However, if they take too long to arrive, then I'll resort to walking.

The issue with Singapore is that the buses arrive at a high enough rate that you don't have to plan your day around catching a certain bus, but slow enough that if you miss a bus, you can be stuck waiting anywhere from five to ten minutes. And as everyone knows, waiting for the next bus, not knowing when it will arrive, is always an experience in balancing tradeoffs especially with an alternative.

While I could do any of the following:

- Visit a website, key in the bus stop number.
- Download an app.

However in reality, the result was:

- LTA's website requests geolocation, but displays the data anyway. It updates in real time. But its a little too heavy, especially if you are on a mobile device/connection because it loads a map of Singapore.

{{< figure src="/images/2020/06/image-5.png" alt="" caption="LTA Bus App" >}}

The SBS transit app renders the page on the server side, and the times are not updated automatically. It also takes awhile to load. Not ideal when you are running for the next bus. (Don't run for the next bus. You can always wait for the next one.)

{{< figure src="/images/2020/06/sbs.JPG" alt="" caption="SBS Transit Web App" >}}

2. I don't see a need to install an app for this one purpose. The apps that are out there are a little too over-engineered for my taste and seem to be variable in how well they work. Plus what if I wanted to check the bus timing from a web browser?

{{< figure src="/images/2020/06/apps.JPG" alt="" caption="Some Apps available" >}}

---

# Ideation

So, after finding all the appropriate excuses not to use an existing solution, let's build something!

I started with Python because it is easy to build a quick prototype. The government agency that handles transport in Singapore (LTA) exposes an API which gives you the real-time data of buses around the island. Lovely, so there's no need to scrape websites!

{{< figure src="/images/2020/03/image.png" alt="" caption="Exactly what we want" >}}

The reason I chose VueJS because it was easy to get started. Just throw in a link to the CDN and BAM, you have Vue on your website. When I was learning React, the main way that everyone started was to use the `create-react-app` package on npm, but this method generates a bunch of boilercode that was a bit intimidating to wade through. I didn't want to do all that. I just wanted to get started. What I didn't know at the time was that you could use a simple `<script>` tag for React to get started like Vue, which in my opinion, is the best way to get started with new technologies.

The challenge for me was to figure out how I could connect a Python script that was grabbing and formatting the data from the API to the website that I was displaying the data with.

One way I briefly considered was to allow the client to talk directly with the Datamall API using Javascript, but I didn't relish publishing the details of my access key to the public so I quickly scrapped that idea.

I was learning some of Google Cloud's offerings at the time and one which really caught my eye was the real-time database offering: Firebase. The idea is simple: a no-SQL hosted database meant for clients that require real time data. When you push data to the database, clients subscribed to the database are automatically notified and updated. In terms of building a real-time app, this was a very attractive proposition, and it helped to simplify a lot of the processes of managing a database. Firestore seemed perfect for my purposes because it was a database where I could upload data to, and it will automatically update any clients which are connected. It saved me some effort on figuring out how I was going to get the data from the front end.

{{< figure src="/images/2020/06/image-3.png" alt="" caption="Network digram" >}}

## Analysis of solution

The main weakness of this solution comes from the fact that Firestore charges by the number of read and writes to the database.

If you consider this system on a larger scale, the limitation of this method is the upper limit on how many reads you can send to Firestore. Each client that is connected to the database counts as one read each time the database is updated. So if you have ten clients that are connected, each time the database is updated, ten reads are being done against the database. This is not scalable.

Another weakness of this solution is that updating the database also counts against the write limit. That sets an upper limit on the number of bus stops/services you can monitor, especially if you are updating the database frequently. Also, since the bus stops to monitor are hardcoded into the Python script, there is no way for clients to request other bus stops.

However, since I am building this for myself, and I am only monitoring one bus stop, these limitations don't apply. :D

# Prototyping

## Getting the data

The first task is to get the data manually. For this, I used Python because it is the language I am most familiar in. I used the `requests` library to send `GET` commands to the API with the appropriate headers set, with `STOP_CODE` set to the appropriate bus stop code:

```Python
headers = {
    'AccountKey': ACC_KEY,
    'accept': 'application/json'
}

path = "http://datamall2.mytransport.sg/ltaodataservice/BusArrivalv2?"
stop = "BusStopCode="+str(STOP_CODE)
path = path+stop

current_request = requests.get(path, headers=headers).json()
```

## Formatting

There is alot of data that is returned on the request so the next step is to convert the information into information that is actually useful.

### Time

I used the `datetime` and `pytz` library to convert the times into local time. I also included some simple error handling because sometimes the time to the stop is also not reported.

```Python
def get_time(value):
    if value == '':
        return 0
    else:
        return datetime.datetime.strptime(value, '%Y-%m-%dT%H:%M:%S%z')

def get_difference_min(future_time):
    tz = pytz.timezone('Asia/Singapore')
    cur_time = datetime.datetime.now(tz=tz)
    try:
        return int((get_time(future_time)-cur_time).total_seconds() / 60)
    except:
        return "NA"
```

The `get_time` function converts the string into a `datetime` object, which is then used in `get_difference_min` to compute the time needed to reach the bus stop.

### Returning useful data

We are only interested in the services and the estimated arrival times. Since the API returns the next 3 buses scheduled to arrive at the bus stop, a dictionary is created with the service number as the key, and the arrival times as a list.

```Python
service_to_stop = {}

for i in current_request['Services']:
    service_to_stop[i["ServiceNo"]] = [
        (i["NextBus"]['EstimatedArrival']),
        (i["NextBus2"]['EstimatedArrival']),
        (i["NextBus3"]['EstimatedArrival'])
    ]
```

The dictionary is then processed through the time functions that were written earlier and returned.

```Python
service_to_stop_min = {}
    for i, val in service_to_stop.items():
        service_to_stop_min[i] = []
        for k in val:
            time_diff = get_difference_min(k)
            if type(time_diff) == int and time_diff <= 0:
                service_to_stop_min[i].append(0)
            elif time_diff != "NA":
                service_to_stop_min[i].append(time_diff)
                
```

Any buses that have a negative time (indicating that they are at the bus stop), are converted to time 0, otherwise, their time to arrive are appended to the list for each service in the dictionary

This dictionary is then returned as part of a function called `get_bus_stops`. However I am interested in organizing the bus arrival by times instead of bus numbers, so I add another function that sorts the dictionary returned from `get_bus_stops`.

```Python
def bus_arrival_order(service_to_stop):
    arrival_order = {}
    for i, val in service_to_stop.items():
        for k in val:
            try:
                arrival_order[str(k)].append(i)  # 3: 975
            except:
                arrival_order[str(k)] = [i]

    for key in sorted(arrival_order.keys()):
        print(arrival_order[key])
    return arrival_order
```

## Firebase Integration

I'm using Firestore as a way to store and distribute the data. After installing the `firebase_admin` package and setting up a project in Firestore, I generated some credentials for my python script using a service account for access.

```Python
import firebase_admin
from firebase_admin import credentials
from firebase_admin import firestore

cred = credentials.Certificate('API_CREDENTIALS.json')
firebase_admin.initialize_app(cred)
db = firestore.client()

```

Tying it all together is pretty simple. Get the data from the API, then write to a collection in the database:

```Python
# Get bus arrival times

arrival_order = bus_arrival_order(get_bus_stops(BUS_STOP_NUMBER))

# Write data to database

doc_ref_arrivals = db.collection(u"bus_stops").document(BUS_STOP_NUMBER+"-arrivals")
doc_ref_arrivals.set(arrival_order)
```

The python script itself lives in a e2-micro instance on gcp, and a cron job is used to call the script as often as the API is updated.

## Displaying the data

Happily enough, the Firebase API also extends to Javascript, so it is just a matter of writing a webpage that will grab and display the data from the database. For this webpage, I am using VueJS.

```Javascript
// Your web app's Firebase configuration
var firebaseConfig = {
apiKey: "AIzaSSDkVtECu22wYZlXr9JHsJ-pkea7hCWmmOA",
authDomain: "bus-stop-553211.firebaseapp.com",
databaseURL: "https://bus-stop-553211.firebaseio.com",
projectId: "bus-stop-553211",
storageBucket: "bus-stop-553211.appspot.com",
messagingSenderId: "25606073122543",
appId: "1:25606073122543:web:cff9f0ad4c4226ba7dakm36b"
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

var db = firebase.firestore();
var _arrival_order;
var BUS_STOP_NUM = 324253

db.collection("bus_stops").doc(`${BUS_STOP_NUM}-arrival`)
    .onSnapshot(function(doc) {
        //console.log("Current data: ", doc.data());
		app.arrival_order = doc.data();
		
    });

var app = new Vue({
      el: '#app',
      data: {
		arrival_order: '',
      }
    })
		  
```

It access the Firestore based on the provided configuration data and retrieves the collection that we have been updating data to via the Python script. The Vue app keeps track of the variable `arrival_order`, and updates the HTML accordingly when the data in `arrival_order` changes.

### HTML

```HTML
<ul>
  <li v-for="(buses,time_to_arrive) in arrival_order">{{time_to_arrive}} minutes:
   <span v-for="bus in buses">{{ bus }} </span> <!-- space after text -->
  </li>
</ul>
```

This is where the magic happens. VueJS allows you to create iterative elements with relatively little code. Here, it generates each `li` element for each time to arrive. Buses that are arriving at the same time are printed together in a `span` element.

Once I got it working on my computer, I uploaded it to a hosting site so that I could access it everywhere.

# Conclusion

After all that work, this is what is generated:

{{< figure src="/images/2020/06/image-4.png" alt="" caption="A list of buses in arrival order!" >}}

Since this caters to the only bus stop I am interested in, I can save this page and access it readily on web or mobile. The time estimates also update automatically due to the client connection with Firestore. It is quite a sparse webpage, without images or icons, and so it loads quickly with minimal overhead.

This is a lot of work for a simple app, but I think I managed to learn quite a bit about how to retrieve data from an API, process it, and push it to a webpage for consumption. I also got to experiment with services from Google GCP.

I think it is a good project to learn with because it pulls together so many different concepts and services. If you can, I encourage you to build a similar app for your city/country!

### Postnote

**SG Nextbus **is the closest web app that I found after I finished development that mirrored closely to what I wanted. The design is clean and simple. Unfortunately it doesn't refresh automatically nor does it allow you to save the bus stop as a link, so you can't jump straight to the page that tells you the bus timings.
