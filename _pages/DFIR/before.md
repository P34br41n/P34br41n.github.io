---
title: Before
---

## What is this about?
What should be done and reviewed all the time. This will be a minimal/simplified version of whatever GRC policy you follow.

## Starter pack
Lets make it simple with no filler and copy pasting, read this:

[https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

Go through everything here:

[https://www.incidentresponse.com/](https://www.incidentresponse.com/)

## Assessing your security levels
Also called maturity assessment, you should run through this every couple of months depending on how your environment evolves. This makes you aware of blind spots in your setup and can be used to talk with higher ups, because it's basically an evolution of graphs and numbers over time... You won't get a perfect score any time soon, but you must strive for it because a *meh, it works, good enough* isn't an option in cybersec...

### Metrics oriented
You have a SIEM and don't know what you see and what you're missing out on? Need to go through your log sources and check around to know that? Need a checklist for an audit? Check out [DeTT&CT](https://github.com/rabobank-cdc/DeTTECT) and [MaGMa UCF](https://www.betaalvereniging.nl/en/safety/magma/) to have a solid base to start with!

### Generalist
You need to setup the how-to's and don't know where to start? Need an action plan that covers a little bit of everything to start out with? Need a reference sheet to know if you're compliant with whatever standard you use? Want to go further and checkout how to implement stuff? Well checkout the [NIST Framework](https://www.nist.gov/cyberframework/framework) and you'll probably get all you need and know where to go next!

### PII & Data
You need to deal with personally identifiable information due to whatever legal constraint you have? CCPA, PIPEDA, GDPR are acronyms that you now have  to deal with and have no idea how to match the *what* and the *how*? Well worry not and start with [MITRE's privacy maturity model](https://www.mitre.org/sites/default/files/publications/pr-19-3384-privacy-maturity-model.pdf) and you'll at least have a sense of direction!

## The Ledger
You need somewhere to keep all of those vulnerabilities/risks you found and what you're going to do about them. This is basically a thing where you're putting info that point to everything you can't see or haven't fixed yet, you might want to keep it securely locked and control access...

### What's in it
You can add whatever custom entry to the table, but the absolute minimum would be
* ID
  * Everything needs one nowadays, go for some CVE style format, or whatever you feel like, but be coherent with it.
* Creation Date
  * Whenever this risk was added to the ledger, this will give you a position in space-time to do stats with later on. You can turn it into a timeline column and build a changelog/evolution of a given risk if you're going for next level stuff.
* Author
  * Whoever added this specific risk to the ledger. It's only there to have a ref and get more details if needed from the entity that thought about it.
* Owner
  * Whoever is tasked with fixing this problem. Either it's one person or a department it doesn't matter, it's just to know who to talk with to know how it's going.
* Risk Description
  * A description of why this is a problem, in details, no generic stuff, no copy pasting...
* Impact
  * What would happen to the company if this happened, in details...
* Mitigation
  * What can be done to mitigate the risk, what's impacted, what's planned and the steps that go with it to fix the problem. You can also add a link to the config files/patches/full description/... to have 100% coverage and know what to do if the same solution can be applied somewhere else!
* Patched Date
  * Whenever this risk gets patched, this will allow you to close the loop and do your stats.

### Risk acceptance
**You don't accept risks, you accept the fact that at a given time you cannot do anything to stop something bad from happening even though you can define it in full details.**

In the meantime, since you know you're vulnerable to something you should dedicate every second you have to find a patch through whatever change needs to be done in your IT/whatever stuff you need to setup/whatever policy needs to be changed/... to lower the risk of it happening/impact it will have, or even better, make it so it *can't* happen. 

You can't find a solution, or it will take some time to be implemented? The bare minimum is to detect and know how to react when it happens. **This is what risk *acceptance* is about**, and on a bigger scale **this is what cybersec is all about**.

Well except if your only defense plan when hacked is to throw a *but loooooooook it's in our ledgerrrrrrr so it's alllllll okaaaaayyyyyy!!!*, then nice one bud, do nothing more and stay vulnerable to your known knowns, might also want to take a break because all of those circle-jerking sessions must be tiring, eh?

## Killswitches
### What
In the physical world, this would be a button you press or a switch you flip to override whatever procedure your stuff is going through and stop that **now**. Of course this isn't built to gracefully stop something, it's only to stop it and know that whatever consequences goes with pulling the plug are probably better than letting it run. 

Which means that if you're going to include an [AZ-5](https://en.wikipedia.org/wiki/Chernobyl_disaster#Reactor_shutdown_and_power_excursion) in your system, you better know exactly what it's going to do to your entire environment before pushing it because stacking it with a somewhat unknown flaw in your system might make it go critical and you probably won't be able to restart it after that...

### Where
Simple stuff, build a [network graph](https://en.wikipedia.org/wiki/Graph_theory) and drop killswitches for the main gates and critical parts of your network. Don't have any? You probably need to setup some network segmentation, so go back to the drawboard first and come back when that's done. Remember that this isn't a *one per ip* thing, it's more of a *define a couple of groups that you can isolate and drop it there* thing. 

Typically you'll need one to cut your Internet access, one to shutdown your datacenter, one to drop your network, and so on... 

### Why
All of the endpoints in your environment are suddenly connecting to weird IPs and acting like they have a mind of their own? Cut the Internet and figure it out!
Stuff is getting exfiltrated massively from your datacenter? Shut it down!
Getting cryptolocked? Drop the network!

Yeah, you get it, it's all about picking the lesser evil and how much you'll lose if you do vs if you don't... But hey it's the last resort and if you've set everything else up like you should, then you probably won't use this anyways.

### How
You need something that you can guarantee will be accessible when it's needed and be clearly labeled as such, because this is basically your last possible plan on the list and it *can't* fail. You might want to do a physical switch, but a virtual one could work in some cases.

You also need to foolproof it because a *we'll just unplug the stuff and call it a day* won't work when the only person in the building is that first week intern that will panic when you tell him to unplug everything...

Whatever you do remember to have enough safeguards on your stuff so that some angry employee that got fired doesn't shut everything down just because he's pissed...

### The process
Everything needs to be defined and stamped by everyone. Phone numbers need to be updated regularly and the *who decides what to flip* needs to be ordered to know who's next in  the killchain if someone doesn't answer. 

Redundancy is the key here if you don't want to get nuked into the closest supernova due to a critical server getting shutdown but no one high enough in the chain calling the shot...

## Incident flowcharts
The more prepared you are, the easier it'll be. If you have a flowchart for everything that can happen then when it happens it'll be a breeze to go through and you won't have to brace for impact since everything will be defined, validated and signed in triplicate. This doesn't mean you can ignore that risk, it means that when SHTF everyone will be ready and have a guideline to follow. 

In this environment flowcharts are supposed to be generic enough to cover multiple scenarios, go for themes and stick to that.  If you need examples you probably haven't been through the [starter pack](#starter-pack) yet and I suggest you do, but remember that you need to adapt everything to your environment before doing anything else! ;)

## DnD sessions
Writing everything down is a thing, going through it and actually following the procedure is another. You do fire-drills in your office? Well do the same with cybersec! Make groups, try stuff out, change things around, mix up roles and actions, automate what needs to be done as much as possible so that when that cryptolocker hits, it's just another day in the office and nothing more. Remember to change your policies, flowcharts, ..., accordingly and to keep it constructive.

Want to go even further? Get your environment pentested/redteamed without telling your team and check out how they react to streamline the whole process a bit more! Whatever you chose to do, don't be a dick though, this should be a constructive exercise not a way to fuck with your team...

## What now?
Go back to the [top of the page](#what-is-this-about) and go through everything again. Simple stuff right?

