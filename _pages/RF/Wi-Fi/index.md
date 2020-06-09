---
title: Wi-Fi
---

# Pre reqs
You'll need some Wi-Fi equipment that supports [monitor mode](https://en.wikipedia.org/wiki/Monitor_mode), which is not the same as [promiscuous mode](https://en.wikipedia.org/wiki/Promiscuous_mode). 
People usually got for Alfa stuff just because, but a cheaper Realtek, Tp-link, Atheros, Netgear, ..., usb dongle will do the trick most of the time. 

All you need to check is if it can switch to monitor mode and the output power it has, which is a simple *the louder you scream the more people will hear and possibly answer*.

You'll probably need [this](https://github.com/aircrack-ng/rtl8812au) too ;)

# Monitor Mode
Just copy paste change the wifi-interface to match yours and be done with it...
```
airmon-ng check kill
ip link set <wifi-interface> down
iw dev <wifi-interface> set type monitor
ip link set <wifi-interface> up
```
You can also set Tx power with
```
iw <wifi-interface> set txpower fixed 3000
```
But you'll end up breaking your stuff if you push it too hard, so RTFM first!!!

# What is this about?
Wi-Fi specific attacks and a little bit more ;)

## WEP
Old-school!

[click me!](./wep)

## WPA/WPA2 4-way handshake attack
Simple warm up here!

[click me!](./4whs)

## PMKID Attack
Simple warm up here too!

[click me!](./pmkid)

## KRACK
つ ◕ _ ◕ ༽つ *SUMMON THE KRAKEN* ༼ つ ◕ _ ◕ ༽つ

[click me!](./krack)

## Evil twin attack
Are you me? Am I you? Can you scream louder than me?

### The Setup
I would recommend buying an [OpenWRT](https://openwrt.org/) compatible Wi-Fi router like one from [GL.inet](https://www.gl-inet.com/), and setting it up so that you just need to flip a switch to start pwning. They're small, can run on powerbanks AND have native OpenWRT support... What more to ask for a cheap version of a [Pineapple](https://shop.hak5.org/products/wifi-pineapple?variant=81044992)?

As of now, and until I update my setup for the walkthrough I usually go for a [https://github.com/wifiphisher/wifiphisher](https://github.com/wifiphisher/wifiphisher) on a [Raspberrypi](https://www.raspberrypi.org/), which is more or less the same. Yes you can easily do it through iptables, some config, rerouting, ..., but hey where's the fun in that?

You can do this with only one Wi-Fi interface, but won't be able to run the deauth part of it, so get at least one extra usb dongle or something...

### PEBKAC
[click me!](./pebkac)

### Client Isolation
[click me!](./client-isolation)


## The Enterprise version

### EAP-TLS
[click me!](./eap-tls)

### LEAP
[click me!](./leap)

### PEAP/EAP-TTLS
[click me!](./peap-eap-ttls)

### PMK-RADIUS
[click me!](./pmk-radius)
