---
title: PMK-RADIUS
---

## What is this about?
Getting the master key from the RADIUS/AP client exchange when it happens, which makes it really simple to do over the air Wi-Fi decryption.

## So what's the pre-reqs here?
This attack involves two data sniffing points, one between the client and the AP by sniffing the Wi-Fi, the other between the AP and the [RADIUS](https://en.wikipedia.org/wiki/RADIUS) server. Yes, this means that you're already on the network... Yes, this probably means you're doing this because you already have access to everything else and thought you'd try it out for the lulz...

## The attack
Getting the PMK + access to RADIUS/AP exchanges basically means that you can decrypt on the fly any Wi-Fi traffic that goes on, and you can bet your ass the secret will never be rotated once in its entire lifetime!

This simply means that if you need months or even years to crack it, it's still worth it because you'll have a clear vision of everything that's happening on the Wi-Fi part of the network!

### Getting the keys
When the AP and the RADIUS talk to each other, they add a hash called *Response Authenticator* to the exchange to be sure that it's legit. This field is built with a simple *[MD5(code + identifier + length + 0000... + request authenticator attributes + shared RADIUS secret)](https://tools.ietf.org/html/rfc2866#page-7)*. Yep, you guessed it, bruteforcing is a possible attack angle here, and MD5 cracking is now highly optimized... 

Every field but the shared RADIUS secret are [known](https://tools.ietf.org/html/rfc2865#page-66) as soon as you're able to get a single pcap of the RADIUS/AP exchange. If you're up against a FreeRADIUS and have access to the server hosting it, checkout [raddb/clients.conf](https://github.com/FreeRADIUS/freeradius-server/blob/master/raddb/clients.conf) to get everything you need! You could also just try *useStrongerSecret* for all you know it might work if no one pointed this out to the admin ;)

### Using the keys.
Since [Wireshark](https://wiki.wireshark.org/radius) has native support for RADIUS you could just go to Edit->Preferences and look for RADIUS in the protocols. Then add your shared secret and start decrypting traffic on the fly.

You can also run tcpdump either of the following ways to start dumping RADIUS traffic
```
tcpdump -c 25 -i eth0 port radius or port radius-acct or port radius-dynauth
# Or
tcpdump -c 25 -i eth0 port 1812 or port 1813 or port 3799
```
Then use [Radsniff](https://freeradius.org/radiusd/man/radsniff.html), which comes with the freeradius-utils package, to decrypt packages with a simple
```
radsniff –x -I <pcap> -s <radius secret>
```
If you're having errors on format, you might want to checkout [editcap](https://www.wireshark.org/docs/man-pages/editcap.html) which comes with Wireshark.

### Why am I doing all this???
Once you have the *shared secret* you can decrypt exchanges between the RADIUS and the AP, which means you can extract the [MS-MPPE-Recv-Key](https://tools.ietf.org/html/rfc2548#section-2.4.3) from the 	*Access-Accept* exchange, which happens when someone tries to connect to the network.

What is the MS-MPPE-Recv-Key you ask? It's a fancy name for the PMK that's used for Wi-Fi encryption. Yes it's [this](https://en.wikipedia.org/wiki/IEEE_802.11i-2004) PMK... Which basically means you're now able to decrypt exchanges between the user and the AP over the air!

To do that, you can start sniffing with Wireshark in [monitor mode](https://wiki.wireshark.org/CaptureSetup/WLAN#Monitor_mode), add the keys in the IEEE 802.11 Protocol and start reading the traffic. Easy right?
