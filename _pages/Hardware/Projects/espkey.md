---
title: ESPKey
---

## What is this about?

Simple, the subject here is [RFIDs](https://en.wikipedia.org/wiki/Access_badge) so pull out your ESP8266 and lets open doors and steal badges!

## RFID Readers
Long story short, checkout the RFID pages to get the info.

## The main stuff
### Wiegand

#### The story
Long story short, Wiegand was an Engineer that discovered that if you put cobalt, iron and vanadium together and made wires out of that alloy, you could switch its polarity when you run it through strong magnetic fields. If you then place a sensor next to it you can get either a low or high voltage pulse that can be translated into 1s and 0s and you get what we call the [Wiegand Effect](https://en.wikipedia.org/wiki/Wiegand_effect).
Then you figure out how to build something out of that, add a couple of wires, standardize it a bit and you get the 26-bit format used in card readers through the [Wiegand Interface & Wiegand protocol](https://en.wikipedia.org/wiki/Wiegand_interface).

#### IRL
To translate that to real life, this means that you have 2 wires to hook, D0 and D1, to get to the data. This also means that we now have a minimum of 4 wires behind the card reader if you add the power and ground wires to the count. 
Of course lets not forget about the other stuff that you can also throw in, like a couple extra wires for LEDs, buzzers, shields, card detection, or even wires that are there just because. With all of that you get your standard badge reader wiring. Note that every wire is color coded though so getting confused is not a thing here.

That being said, the interesting stuff is on the D0 and D1 wires since that's where the data on the cards that are read will transit _directly & unencrypted_ to the central system that will check if the card is valid or not and unlock the door. This transfer can be done through Wi-Fi, or by other means, if you need more than 150m of wire between the reader and the secure closet with the brain, but that's another story.

### RS-485

#### The story
RS-485, just like Wiegand, is a standard that defines how things should transit on the wires. The difference is that we're now talking about chaining things on the wires and about a 1.2km range. This means that instead of doing a direct link between the reader and the access controller in a star pattern, like in Wiegand, you can pull a single wire between each reader and at one end have your controller.

#### IRL
Technically speaking, RS-485 can support multiple devices on the same bus (32 nodes or 256 if you're on a high impedance input setup) and you also have 2 signal lines that you can tap & pipe through your oscilloscope or analyzer, like a generic RS-485 to USB dongle, and get what you need to open a door somewhere. But this is the thing, you don't go for a star pattern and the longer the chain, the slower the speed starting at somewhere above a futuristic 30Mbps down to an oldschool 56kbps.

### OSDP

#### The story
Some time between the 80s (with Wiegand) and now, some dudes were all _*hits blunt* Hey bruh, we can send things from plastic cards over metal wires to open doors 'n stuff, but what if we encrypted the data with the shit they use for websites, make a new standard and sell that for cash?!_, and long story short a couple of encrypted protocols happened, including OSDP.

#### IRL
Open Supervised Device Protocol (v2) is basically a way to AES-128 the data you want to send over RS-485, or in other words, you can tap that but you ain't getting no syrup. 
Fun story, checkout the date on _IEC 60839-11-5_, and then the concept of hardware updates like _OSDP converters_ and think about all of that for a bit. :D

### The other stuff
There are a bucket load of communication protocols (RS-232, RS-422, Wiegand, Ethernet, Bluetooth, Wi-Fi, ... ) and the same goes for the encryption part (OSDP, SSCP, SSCPv2, ...), but at the end of the day you're sending data going from a card reader to a controller and it's possibly encrypted. Just like the upgrade of http -> https basically.

## The build
### What are we doing?
Since a most of the badge readers use either Wiegand or RS-485 with no encryption you _can_ tap the data wires and extract/replay badges. If some form of encryption is setup, most of the time it's a converter and you can _still_ tap the wires between the reader and the converter. If the encryption is built in, well too bad.

### Battle plan
We need a couple of things to make this work, which means we're going to split this up to make it easier to go through:

- A way to get to the thing that does the stuff
- Get stuff off the wires
- Push stuff on the wires
- Store stuff

Clear and easy steps isn't it?

## The code
The pre-reqs here are simple: HID, Wiegand, no encryption. If you have another physical layer setup, just use the matching library, if it's encrypted, you're back to sniffing traffic, cloning cards or finding the key (pro-tip, it's somewhere in the security closer on the controller).
Most of the libraries used here are pretty standard and will come from [this repo](https://github.com/esp8266), for the Wiegand/RS-485 just look for libraries on github or read up on it and code it yourself. 
At the end of the day you could also skip all of this and go grab a repo like [this one](https://github.com/octosavvi/ESPKey) and push it on your ESP8266, but there's no fun in that.

### A way to get to the thing that does the stuff
You could go and physically get your thing, connect through bluetooth, send text message with an LTE setup, but since it's an ESP8266 we're going to use Wi-Fi. Now the thing about Wi-Fi is that if you setup an AP anyone can see it, you can put a random BSSID/ESSID, you can spoof one of the IDs around, you can always disable broadcasting the name, but anyone with a pro Wi-Fi setup will see your stuff and might even get an alert somewhere about it. Then you're just a dBm [triangulation](https://en.wikipedia.org/wiki/Triangulation) away from losing your thing.

The other way to do it is if your thing connects to your AP.
-> Can you see that something is trying to connect to a Wi-Fi? Yes, and you can also see the name. 
-> Are you going to have an alert somewhere for that? Usually, no.
-> Why? Imagine Bob's phone, he connects his phone to his home Wi-Fi, the Airport's Wi-Fi, his 6 friend's Wi-Fi, ... and since he turn the auto connect on, well his phone is broadcasting that info all day.

#### The detailed version
That being said we're going to need to include a couple of things from the Arduino libs.
```
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h> 
```
You could also replace those with ESP8266WebServerSecure.h and WiFiClientSecure.h but adding cert management won't be covered here since we're assuming you're going to be pretty much alone on the Wi-Fi and I'm too lazy to redeploy code with valid certs every year (even though you could self-gen a cert valid 420 years or something and be done with it, but that's not the point, just focus on the _I'm lazy_ part).

#### TL;DR
```
Coming soon!
```
### Get stuff off the wires

### Push stuff on the wires

### Store stuff



### The setup


