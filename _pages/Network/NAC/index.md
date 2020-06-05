---
layout: posts
title: NAC
---

# What is this about?
Network Access Control, which means if you're X you get access to this and if you're Y you get access to that. Define X and Y you say? Simple, X and Y are defined by something that you can match on, this can be MAC addresses, tokens, keys, certificates, emails, ... or whatever else you can think of.

Ofc the more unique and harder to tamper with the better. For example, if you set your filtering on MAC addresses and 2 endpoints happen to have the same one (lets ignore the bundle of network problems that come with that for a second), they will both match the same rule and end up in the same place with the same access rights. 

Sure you can match on a partial MAC address to get [some info](https://macvendors.com/) on the thing that's connecting to your network, to separate endpoints and servers from your everyday top of the line [IoT toaster and trashcan](https://lmgtfy.com/?q=iot+toaster+and+trashcan), but this is easily spoofable and bypassable.

If only you could give out something unique to each user/endpoint/server/... and match on that... Something that you could generate and hold total control over, like a key to open then front door of your house, or a passport to prove that you are who you say you are... 


## Your cryptic stuff is lame, get on with it...
Alright, alright, here are the goods

```
Coming soon!
```
[PacketFence](./packetfence)