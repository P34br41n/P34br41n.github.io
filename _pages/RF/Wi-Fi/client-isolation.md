---
title: Client/AP Isolation
---

## What is this about?
Dropping a common security setting on guest networks and start poking around.

## What are you talking about?
Client isolation or AP isolation is a setting on your hotspot that you can turn on to isolate connected clients so that they can't communicate with/see others. Which is a nice thing to turn on when you don't know/trust everyone on your network, say for a guest Wi-Fi.

But since you control the settings, why turn that on?

## The game plan
This can be done by simply turning on any Wi-Fi AP that clones the properties and not checking the network isolation box, but this wouldn't be fun. So lets also go through a random [tool](https://github.com/InfamousSYN/rogue) you can use so that you'll have a starterpack for other things you could do!

### Step 0
Read [this](https://github.com/InfamousSYN/rogue/wiki/Performing-Attacks)

### What now?
We're not going for sslsplit attacks or phishing attacks, we're going for the responder one, but with a twist.

For starters, lets run a basic instance of rogue. You can specify whichever protocol/auth you want to use but lets do a simple version and go for an open network.
There are a couple of ways to config the tool, either modify the default [setup.py](https://github.com/InfamousSYN/rogue/blob/master/config.py) or just run with options. You can use the responder option but this is limited to specific environments.

To start this you want to clone the target's channel + ESSID + BSSID and relay whatever to the actual Internet through your computer's Internet connection, then you just need to run a 
```
python rogue.py -i <potato-interface> -c <target channel> -e <target ESSID> -b <target BSSID> --auth open --internet
```
Once up you can play the deauth game or simply wait for someone to connect. In the meantime you could RTFM ;)

### Someone connected!
Good! Since you're also on the network and there is no client isolation, start your [Responder](https://github.com/lgandx/Responder) and wait for, or force, someone to connect to something that needs NTLM auth to get some hashes. 
```
./Responder.py -I <your interface> -rPv
```
Ezpz!

### What that's it?
Well if you have some extra time on your hands and already have access to the internal network, you could set it up to go for [SMBRelay attacks](https://en.wikipedia.org/wiki/SMBRelay) and target admins or whatever other [PtH](https://en.wikipedia.org/wiki/Pass_the_hash) scenario you want. 
