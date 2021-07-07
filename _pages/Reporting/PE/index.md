---
title: Pentesting Engagement
---

## What is this about?
The stuff that should be in a pentesting report. There are a couple of examples [here](https://github.com/juliocesarfort/public-pentesting-reports) and more stuff [there](http://www.pentest-standard.org/index.php/PTES_Technical_Guidelines#Reporting) if you need some visuals. 

### Paperwork
This will need have as much info as possible on what the engagement will be about. Since this a contract between two parties a lawyer needs to go over it because when SHTF this will pop up and anything not in  here will bite you in the ass.

#### Contacts, authors, ...
Anyone that will be involved in the engagement, on both sides, should have all of their info added here. If something happens this will be the reference for who to call/email/... and who the pentester was on your side.

#### Scope
This defines what you're allowed to play with, either it's urls, ips, hash values of files, ... Anything not there should be considered as a *will get you in trouble if touched* thing. This is also something you can refer to if the client gets pushy with free pentesting stuff, as in, you can just throw out a *Yeah, sorry but legally I can't do anything else than what's written on the engagement paper because if anything happens, you know, I'd be in trouble, you'd be in trouble your insurance won't cover it, ... So yeah, sorry but I can't. Best I can do is contact my superior to plan for another engagement if you really need more tests to be ran*.

#### Rules of engagement
Sometimes there are work hours related restrictions, or only certain types of attacks that should/shouldn't be done, or maybe you're going for a full on redteam but social engineering isn't allowed on personnel that isn't in targeted department because they're subcontractors. This also works the other way around, for example if you're supposed to send equipment over and the way to start it, how it's supposed to be handled, what it will test, ... You'll also need to have a flag you need to go for, which means you need to define what the client is ok and not ok with.

#### Engagement dates/timeframe
Of course, like everything, there is a start date and an end date for any given perimeter you need to pentest. This should be written down explicitly for legal reasons and if anything happens outside of those timeframes, then it probably isn't you...

#### Get out of jail card
This is mostly for physical pentesting and you'll need an extra copy per person, but hey, it's always a good thing to add to the contract that everyone is OK with what's going to happen and that your life won't be nuked because someone changed his mind...

#### Methodology used
OWASP, OSSTMM, PTES, PCI-DSS, NIST800-115, ISSAF, some random thing you made up? The standard you'll be using should be added somewhere so that everyone knows what will be going on during the pentest and has an idea of what might come out of it. Don't be lazy and write that you're following the WSTG on a physical pentest though... This is where you need templates and organization...

This part also involves explaining any ranking system that you will use later on.

#### List of credentials
You're going for a grey/whitebox? You probably have some credentials to test your stuff with, and you'll want to write these down here so that everyone is OK with what's going to happen. If full info isn't possible, at least a *3 accounts will be created specifically for the tests, 2 basic user accounts called guest1 and guest2, and a low privileged moderator account called cleaner1* should be written down. For clarity, just go for a table.

|Account|Password|Access type|
|---|---|---|
|guest1|abc123|basic access|
|guest2|123abc|basic access|
|cleaner1|a1b2c3|limited moderator|

#### Responsibility clauses
This will be a give and take between legal departments, but it boils down to *if things break who's responsible*? This is where asses are covered, where sentences like *please do a full backup copy of the system beforehand so that you can restore it if anything goes wrong. Know that our trained professionals will do everything they can to avoid breaking your environment, and will contact you if they deem that the tests they will be doing might break something. Following that you will be able to give your validation or refusal to continue in that direction. If you refuse, know that the tests will not be ran, and therefor that we will not be able to check if your environment is not at risk of an exploitable vulnerability that may be found by a third party in the part of the scope that wasn't tested*.
**FIY, pentests do not guarantee 100% coverage nor does paying for one guarantee results. You should never write anything that says otherwise or you will get fucked hard in return...**

### Common part
This is what's in common between the tech and exec reports, it won't be written the same way but it'll be the same info.

#### Vuln ID
Find an ID format and stick to it. Either it's *V-1*, *VU#01*, *Vuln-01*, *Vulnerability-XSS-01* or whatever else, if you're not consistent on everything you fill up, the readers will get confused and you might lose that client because there will be no link in between the different parts.

#### Vuln score/Difficulty to patch
Either you're obsessed with meaningless numbers and use CVSS or just go for a minimal *critical/high/medium/low*, pick one and stick to it when explaining anything vulnerability related. This is also valid for the patching steps, sure you can write *can be patched in 3 nanoseconds by a team of highly train goldfishes* but hey, it's just a guess anyways, you won't get it right either. 

If you're unsure, what you should do is give yourself some maneuverability and stick to the *short/medium/long/...* ref system, since this will go through the entire internal politics process you'll never get it right either anyway.

Whatever you do remember that this is always a subjective rating based on your skills and that experience will tend to make everything shift to *trivial* exploits even though a noob will find it impossible to exploit.

#### Attack scenario
This scenario is a plausible story you can make up from chaining all the vulns you found. It doesn't need to be 3 pages long, a simple example would be *The entry point is here, then lateral movement through this vuln, privilege escalation by stealing session cookies, more lateral movement to get to that part of the scope, server access through file upload and LFI, server compromise due to highly privilege account used by this vulnerable service. Following this a network scan and breach of other servers due to identical accounts/passwords used. Finally pivot on this server to get to that one, which hosts your internal profile/chat system. deployment of a modified version of the login page that stores login credentials in cleartext in a file and domain admin credentials captured, Golden Ticket pulled. Good luck next time*.

Just write a cool story with a plausible scenario, got it?

#### Summary
The summary should not be a second version of the attack scenario, you should give a quick evaluation of the security levels of the scope and compare it to security standards. This involves sentences like *The scope defined was easily breached and no automated response was seen during the tests. Your response team didn't respond properly and some systems that were used to get in are now EOL and trivially exploitable*.

Yes, sometimes you're also testing the team that's supposed to respond too...

### Tech
The tech part should be full on tech language, no simplification, no *things and thingies* and so on. You're talking to a tech and he's supposed to know what you're talking about.

#### PoC/Steps to reproduce to test
Any vulnerability sheet should have a proof of concept with it. Either it's problems your nmap scans picked up or your *super 1337 XSS/RCE combo chain in one click*, this is here so that when the vulnerability needs to be tested all of the steps will be known and it's basically just a walkthrough. 

You'll thank yourself when you come back to do a follow-up on a pentest you did 3 years ago to check if everything has been patched... Been there, done that, your super duper chained combo will be nowhere to be found and you'll waste some time rewriting it.

#### Screenshots
Screenshots should be targeted, no extra info should be on them and no PII should be visible. You can blur stuff out or just drop a black/red/green/... rectangle on it or part of it, whichever thing you like as long as readers can't see through/guess and you hide enough info... 

Arrows are also a thing, either red, yellow or green are okay, going for fancy colors where you need to explain the difference between teal and petrol-green isn't, so don't. Remember that everything should be kept to a minimum, if you need to plaster half of your screenshot you might want to take another one. You should also screenshot your stuff and then work on a copy of it, because you might think you don't need that part that you cut out, but sometimes it'll save your ass. 

For example, you take a screenshot of the homepage of a website and cut out the login part at the top, but on the bottom there is the version of the deployed website composed of the day/month/year/... of the deployment and when you come back after the weekend you notice that your vulns have disappeared even though the devs swear nothing has been pushed and that the contract states that nothing should be changed in the code during the tests. Except that they fucked up and forgot about the versioning system that gets updated every time something is pushed. Anything you do from there can be covered by a simple *you changed stuff again*, it's also a breach of contract, but that's another story.

So yeah 100% *screenshot or it didn't happen*.

#### Recommendations
There are two ways to do this, and this is where your internal database and knowledge of the clients infrastructure will be extremely valuable.

The first way would be by giving a generic patching recommendation and advices like *Apply vendor's official patch* or *You need to remove TLS 1.0 config because you are not PCI-DSS compliant*.

The second way would be either tech specific or even better, client specific recommendations, as in *Since you have a global infrastructure modernization project that will involve adding proxies and access control internally, you could temporarily add an extra rule and hook this service until it becomes redundant with the new project that covers the same functionalities*.

This will be greatly appreciated by your client because going for a *you have to change the source code and use OWASP libraries* won't fly when it's running in COBOL...

#### Redacting information
You will at some point have to take a screenshot with sensitive information, PII, passwords, ..., visible which you quite probably will want to redact from the final draft. This is basically done according to your personal taste and what you're okay and not okay with. But see it like this, if your report leaks, or if a random employee stumbles on it for whatever reason, how much of your target's info are you okay with being out there?

#### Exfiltrated stuff
You'll always need to show some proof that you had access to a server, database, AD, ..., because some smartass will always play the blame game over what you did and didn't do even though everything was defined before the engagement started.

How much of those databases/creds/... you want to pull as evidence of what can be done is up to you. But this is the thing, except if you know that MR. Smartass will be somewhere in the audience at some point, why the hell would you pull TBs of data from everything you can get your hands on? Why go through every laptop in the company and take a camera pic as proof when you can just go for the C levels and get the exact same effect?

If *because I can* is your answer, welcome to the  team, but it's just going to be more stuff that takes up space and that you'll need to erase after the engagement so don't go overboard...

### Exec
Executive reporting is totally on the other side of the fence. No tech stuff, as much simplification as possible, matrices, graphs, stats, ..., the whole GRC package basically.

#### Global overview
This should be an excerpt of what you've noticed was good and bad during the tests. Stuff like *noticed that accounts were locked for 15 mins after 5 incorrect attempts, which means that the standard best practices are followed correctly*, or *after getting on the server, there were indications that the network's architecture was flat. This type of architecture facilitates movement on the network following a breach and it is strongly recommended to add complexity to the network. Since this was out of scope no further tests were done*, should be added here. Don't forget that this is a non technical report though.

#### Vuln matrix
The vulnerability matrix is built with **attack complexity** / **vuln criticity**. You can put numbers, words, smileys, colors, whatever else, but that is what it should show. For a quick read the matrix should be kept small, with 4 or 5 levels of complexity/criticity. For example, easiest to exploit/higher criticity top left would look like this.

|---|---|---|---|---|---|
|||critical|high|medium|low|
||trivial|*total shitshow*|||*probably a bot*|
||easy|||||
||medium|||||
||hard|*you're targeted*|||*meh*|

You can also reverse it and have the most critical top right instead of left, but hey your choice. When you fill it up, you're going to only drop the vuln-ID where it belongs. Which means that filling matrices up can and should be automated because it's a waste of time to do it by hand.

Friendly note: Matrices don't mean much and a _medium_ vuln can wreck your entire shit a lot more than a high one sometimes, but hey who reads these aside from paper pushers anyway?

#### Risk matrix
The risk matrix is build around **probability** / **impact**, this is just another view for the vuln matrix, but from the GRC position. Vulnerabilities don't exist here, only risks and impacts... You can use words like *probably*, *frequently*, *likely*, ..., but sticking to the basics is possibly better here, and of course the scale depends on the scope. If we go for a high probability/critical impact top left, this is what it would look like

|---|---|---|---|---|---|
|||critical|high|medium|low|
||high|*cryptolocked*|||*Leaky faucet*|
||medium|||||
||low|||||
||rare|*shit happens*|||*will be ignored*| 

The fun thing about this table is that the viewed goal is to push everything as low/rare as possible instead of patching it properly. This is the whole *theoretical* part of cybersec, which is quite often disconnected from how it's done... Filling the matrix up should also be automated.

Friendly note: Matrices don't mean much and a _medium_ risk can wreck your entire shit a lot more than a high one sometimes, but hey who reads these aside from paper pushers anyway?

#### Patching matrix
The patching matrix is built with **patching complexity** / **time needed to patch**. Cash spent is also the main idea here. This matrix will be entirely subjective and the more you know the infrastructure/code/workforce/..., the better you can approximate it. But hey, except if you know 100% of everything you'll get it wrong so keep it simple. If we set it up as a trivial to patch/short amount of time to do so, this is it

|---|---|---|---|---|---|
|||short|medium|mid|long|
||trivial|*apply official patch*|||*herding cats*|
||easy|||||
||medium|||||
||hard|*code properly ;)*|||*won't happen*|

Fun part about the *hard to patch/long to patch* one, you'll come back years later and it won't be fixed either because the patching project is still ongoing like for a complete network architecture overhaul, or a lot more often because the *cost to patch is higher than the loss if exploited so we accepted the risk and moved on*...

#### Roadmaps
Keep it short and global, go for a step by step walkthrough as much as possible and don't overload the map. Think of it as a git tree that you can easily go through even though there are a couple of branches and merges. This should be ordered by shortest/most trivial to hardest/longest to patch, the twist is just that you can go for a higher level patching, like architecture change or a patch for multiple vulns at once.

#### Quick Wins and temporary fixes
This is what should pop out the most and what has the best security gain ratio. It's stuff that can be done quickly and that covers most ground, for example
* SQLI in search bar? Deactivate functionality until a patch can be applied.
* Got a vulnerable website? Drop a WAF in front, patch it ASAP and switch it out.
* Your perimeter is being regularly tested by bots in whatever country? You're tired of hearing your security team ask for something to be done and have no customers over there? [Geofence](https://en.wikipedia.org/wiki/Geo-fence) it out! Might also throw in every VPN, TOR exit node, ..., to that list too!

You get the point... Remember that everything should be properly patched and secured and that this is just a list of *do it to lower the risk* not a *do it to fix the problem*, since the real patches will be in the tech report.

### Extra
#### Nessus/OpenVASnmap/... scan
It's an extra thing to give with your report since you can't go over everything anyway and you'll have to skip over some, but if this is all you give and ask to be paid for your *hard work* of clicking on a webpage to launch a scan, well there's a special place in hell for you. Sure your profit margin is out the roof by doing that, but that's taking a toll on the entire security community and it's clearly visible to everyone that you're just into it for the cash part of the engagement, so except if this  is what the  client is explicitly asking for, fucking don't...

### Exit meeting
Once everything is all wrapped up, and the report is sent, an exit meeting is usually set up to talk about the whole pentest.

There are a couple of scenarios here, the main ones are the one where you're working for a client and talking about his homebuilt stuff, and the one where you're working for a client and talking about a third party that's being paid to build something for your client. 

The first scenario should be a standard meeting explanation, etc. The second one is much trickier since, in a way, you're going to shit talk the work of a third party that will probably be there and you're getting paid to do so. Which means that they might forfeit all of their bonuses and might even get a non-negligible cut off of their salary depending on your work. But hey they're not paying you and should have done it properly in the first place, right?

#### Attack scenatio
If you've done everything else properly, you're just going to quickly go over the most critical stuff you found and follow the story of how your client will possibly get breached. Except this time you can choose the way the story is told depending on the audience. Go for one screenshot per slide, possibly integrate a screenrecording or two, but keep everything short, everything is in the report anyway. Do take some time on questions and expect resistance from anyone in front because you've broken someone's stuff and ego comes into play...

#### Screen recordings
You should keep every recording handy and ready to play, but you shouldn't play everything because no one in the room probably cares about seeing a live action replay of what you did. 

This is the thing, as soon as someone pulls the *that isn't possible because I did X and that blocks what you did, so you're lying* card, just drop a *Well, lets watch it live then! I'll even throw in the commentary so that everyone in the room understands what is going on* counter, follow through with it and give as much details as possible on everything. It'll get patched correctly that way and there will be less unnecessary interruptions. 

Don't be a dick though, some questions might be legit and you want to stay constructive...
