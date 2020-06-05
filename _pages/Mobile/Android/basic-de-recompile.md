---
title: Basic decompiling and recompiling apks
---

## What is this about?
You have an app on your phone, but you want to change something in it to make it "better" or just because you need to pentest it and that certificate pinning stuff is blocking you or you want to change the way the app works. Well worry not! Everything can be reversed, messed with and rebuilt!

Fun story, a couple years back I was interviewing for a generic pentester position and one of the senior techs was grilling me on how to pentest Android/apk/..., what to look at, what's considered a security issue, what to do to fix stuff and so on. Then he more or less told me it wasn't possible to decompile, change and recompile an apk when I started explaining how to bypass/drop certificate pinning on a client's app to simplify proxying and sniffing stuff. 

This might happen to you too! If it does, just bail tf out of the interview ASAFP in any way possible and find somewhere else to work at, because this is the same type of reasoning behind the whole "We've never been hacked and my stuff is unhackable anyway, I don't need to get it pentested"...


## Dude your life story isn't interesting!
Ok bud, Here's the stuff you're looking for...

### Step one, the Tools
Check these out and go through the options!

[ADB](https://developer.android.com/studio/command-line/adb)
[Apktool](https://github.com/iBotPeaches/Apktool)
[Dex2jar](https://github.com/pxb1988/dex2jar)
[Jarsigner](https://openjdk.java.net/tools/index.html)
[A decompiler](http://www.javadecompilers.com/)
[Keytool](https://openjdk.java.net/tools/index.html)
[Zipalign](https://developer.android.com/studio/command-line/zipalign)

You might also want to check out the [APK Signature Scheme](https://source.android.com/security/apksigning) and [apksigner](https://developer.android.com/studio/command-line/apksigner.html) depending on what you're doing. I'll stay on the basics here and go with "oldschool" jarsigner.

Note that if you're going to use apksigner instead of jarsigner, you need to zipalign before signing not after...

### Step two

#### The theory
Use apktool on your app.apk to get the manifest.xml, folders and .dex files
Use dex2jar on your .dex files to turn them into .class files
Use your favorite decompiler to "inspect" the code
Use apktool again to rebuild the apk
Generate a key with keytool to sign the apk, if you need a new one
Use jarsigner to sign the apk
Use zipalign to align everything so it can be loaded

You just need to run *adb install your-new-app.apk* to push it to your phone, and you're done! 


#### The things you're gonna copy pasta
```bash
apktool d app.apk
#dex2jar can also be ran directly on the apk if you want to...
dex2jar app/your_stuff.dex
#Decompiling step goes here, pick your tool and RTFM ;)
apktool b your_modded_app
keytool keytool -genkey -keyalg <potentially RSA> -keysize <Pick one> -alias <your alias> -keystore <your keystore>
jarsigner -sigalg <potentially SHA1withRSA> -digestalg <Potentially SHA1> -keystore <your keystore> your-new-app.apk <your alias>
#You can check if everything is OK with the jarsigner -verify option, if you need/want to
zipalign -v 4 your-new-app.apk zipaligned-app.apk

adb install zipaligned-app.apk
```
And you're all setup to click at least treefiddy times quicker on your cookie clicker now ;)


#### Nice, but what is this good for?
Well lets say you have to pentest an app and don't have anything but some random phone laying around. Your computer already has VMs inside of VMs and can't handle emulating Android on another one, your Frida hooking skills are meh at best and you notice that there is certificate pinning in the app and adding a cert to your trusted store doesn't work.

Which means you're gonna have to either replace the the pinned cert with your own or patch the code that checks the pinned cert. Which is where all of this comes in!