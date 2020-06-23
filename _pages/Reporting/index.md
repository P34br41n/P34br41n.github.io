---
title: Reporting
---

# What is this about?
Reporting templates, matrices, tables, GRC stuff, ..., will end up here.

## The general idea
So you found a couple of vulns and now you have to send it to someone with details, PoCs recommendations on how to fix it, stats, graphs, matrices, ... But have no idea what you should put together? Worry not! This page is here to help!

When you build a report there are many things to take into account other than what you put in it, for example who's going to *read it* and the *context*. Newsflash, if your report is ever used in a court of law, you can bet your ass whatever exploit/PoC/proof/... will be questioned because you made a mistake in a footer or took a lazy screenshot without whatever URL/date/... visible and had to add it later by hand...

## The R&D surrounding it
If you plan on doing high quality reporting and spending the least amount of time possible on that, because we all know the paperwork is the most boring part of the job, just like everything else you'll need the tools to do it.

### Environment builders
This is about cutting the repetitive tasks out and automating stuff like creating one folder per mission, setting your screenshot auto save path to a specific subfolder, creating a note taking db for that mission and locking it with a unique password, generating those daily-report-yyyy-mm-dd.txt files with the [ASCII art](https://en.wikipedia.org/wiki/ASCII_art) headers and dropping them into a subfolder, ...

Be lazy, write the script once, run it when the mission starts and stop wasting some time on uninteresting stuff...

### Knowledge database
This can exist in many forms but it's basically a database that gets filled up with generic vulnerability templates, recommendations, auto-filling tables and matrices, risk score, ... To have a homogenized base to work with. 

Then you can start creating client/scenario specific variants. 

### Notes
Got a list of passwords and need to put them somewhere you can access? Don't know where to put that nmap scan result? Guess you need [note taking](https://www.giuspen.com/cherrytree/) [stuff](https://github.com/laurent22/joplin) then... Don't forget the DB password, you never know ;)

### Screenshots
Yes, your OS screenshot system is enough, but hey we've all been through MSPaint to hide PII and add arrows, but there's a better way... Checkout [Flameshot](https://github.com/lupoDharkael/flameshot) and [Greenshot](https://getgreenshot.org/) for starters. 

Now the *how* to take a screenshot is another thing you should play around with. But basically you need to focus on what you want the reader to see and keep text size consistency as much as possible. If it's not readable or hilariously big, you're going to break the flow of the document...

### Screen recording
If you've ever been part of a pentest exit meeting, you probably know about the reluctant consultant/dev/tech lead/manager/... that doesn't want to admit that they/their team fucked up, or that random admin that swears was told to not block whatever you did but obviously didn't see shit and is the source of the last 5 breaches...

Fun stuff, you know the whole *playing chess with a pigeon* thing? A simple *Screenshot or it didn't happen* solution usually fixes that and a screen recording is like many screenshots one after the other! This also allows you to explain everything properly with visual support.

## What you should get insta-nuked for in 10 examples
* Sending a report with another client's PII in it.
* Running a Nessus, exporting the scan and calling it a day.
* Pentest **and** fix the problems, or vice-versa, this is a conflict of interest.
* Screenshots with that shitty neon green colorized bash...
* ***xxbigdhaxor69xx***@***pleasurepod***:\~#... Seen some, fun times...
* That bookmark bar with your favorite porn links? Yeah, you better hide it.
* The wallpaper with your waifu or some stupid shit.
* Anything on *The World's Worst Penetration Test Report by #ScumbagPenTester*. ;)
* Leaving any stuff you used on the target's network
* I've already got 25 vulns, I'm just gonna take it slow and wait for the end of the engagement now...

## The split
There are two types of people who will read your report
* Techs that will need the PoC, steps to reproduce, code and config to patch, ...
* Non-techs that will want numbers, graphs, stats, timetables, ...

If you give a fully technical report to executives they'll quite possibly not read it, so you have to basically go for a "You're fucked if you don't patch X, Y, Z in that order and it should be a quick/long patching process to do so".

If you give an executive report to a tech, he'll probably say something like "Fuck is this? What are the things to patch? Where is the vuln precisely? This is useless to me!", so you also need to give a report with the tech side of it.

Lets split the main *what goes where* parts by who it's useful to and also split depending on what you're reporting, but you probably guessed that right?

### Pentesting engagement
[click me!](./PE/)

### Incident response engagement
[click me!](./IRE/)
