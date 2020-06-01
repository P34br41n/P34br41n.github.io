---
title: TLS
---

## What is this about?
Upping your security game through better web security

This won't be a monologue about RSA vs ECDSA vs Ed25519, or why the only answer to "but my grandma uses [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape) and I need that [S-HTTP](https://www.ietf.org/rfc/rfc2660.txt) option for my custom things at home", should be "lol, fuck off".

Fun story, I had to pentest a "we forgot about this for 20 years, checkout the spiderwebs on it!" website serving "S-HTTP" in another life, which lead me to recoding a basic browser that would talk with it... Fun times with security by obsolescence! :p


## Dude I don't care about your lifestory
Alright, alright, here are the goods

### TLS configurartion generator
Use this to generate your config file for whichever software you use to serve your website.

The why is here:

[https://wiki.mozilla.org/Security/Server_Side_TLS](https://wiki.mozilla.org/Security/Server_Side_TLS)

The how is there:

[https://ssl-config.mozilla.org/](https://ssl-config.mozilla.org/)


### Testing your configuration
If you're thinking "nah I'm good no need to do this", just run a simple [testssl.sh](https://testssl.sh/) on your website and start from there ;)