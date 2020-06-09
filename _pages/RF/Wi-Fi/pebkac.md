---
title: PEBKAC!
---

## What is this about?
We're going phishing for creds through Wi-Fi!
For simplicity we're going to stick to the basic scenarios offered by wiphisher. Know that these should be customized to the target for a higher chance of it working. 

No, I will not be sharing my templates, modules, scenarios and stuff here :p

### Step 0
Read [this](https://wifiphisher.readthedocs.io/en/latest/extensions.html) and [that](https://wifiphisher.readthedocs.io/en/latest/custom_phishing_scenario.html) and you should be good to build your own stuff.

### The examples
Here are a couple of example given by wiphisher to show what it's able to do. Don't be stupid and start clicking things and entering your creds now...

[Firmware-update](https://wifiphisher.org/ps/firmware-upgrade/)

[Plugin_update](https://wifiphisher.org/ps/plugin_update/)

[OAuth-login](https://wifiphisher.org/ps/oauth-login/)

[Wi-Fi-connection](https://wifiphisher.org/ps/wifi_connect/)

### OK what now?
Know your target and play around with the examples.

The OAuth is close enough to what I usually use/do when going for a breach since most companies now have some level of SSO/MFA combo to log on to everything. This means you'll need a multistep login system that validates the given creds by sending them to the legit login page, copies what needs to be copied and forwards the user to where he thinks he's going. You're now getting access to everything a user can access.

The Wi-Fi-connection is perfect for companies that don't have certificate authentication. Network disconnects happen and users will try to reconnect. You could also make it weird and ask people to upload their cert to be checked, who knows it might work ;)

You could push even further and change the Plugin_update to a "due to reasons you need to install this root cert on your computer to access Internet" and then you're basically going to do traffic decryption, but you'll need another setup for this.
This is also where you can push payloads to the user's computer, test the browser itself, run [BeEF](https://beefproject.com/), run some websocket portscanning, ..., or go for some more exotic stuff like [this CVE](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2020-0601)!

You could also mix things up and clone the captive portal of the "guest" Wi-Fi and run that, which is usually where the phones and personal stuff of the employees are connected to.

You're limited by your imagination I guess...