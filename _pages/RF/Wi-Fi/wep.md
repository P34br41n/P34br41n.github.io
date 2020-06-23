---
title: WEP
---

## What is this about?
I'm sorry but are you from the past?  

## Step 0

Read [this](https://en.wikipedia.org/wiki/Wired_Equivalent_Privacy).

## Lets one shot this
There are a couple of different attacks on WEP, which basically all result in RC4 problems. Since WEP is not that common anymore, this won't be the whole spam packets to get the IV through multiple terminals of aircrack-ng tools with a multi-step walkthrough, all the info is on the wiki so this is going to be a oneliner.

Setup your monitoring interface and run the following command, everything comes with [aircrack](https://github.com/aircrack-ng/aircrack-ng/tree/master/src/wesside-ng)
```
wesside-ng -v the-target-BSSID -i mon0
```
This is going to be a couple of minutes long tops, it will drop the password on your screen and you're done.