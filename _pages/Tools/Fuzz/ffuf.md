---
title: ffuf
---

## What is this about?
Fast web fuzzer written in Go

Link: [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

### Basics
Looking for a page on your generic PHP website but don't know where it is? Need to do this while authenticated? DirBuster is just not your thing? Just run a:
```
ffuf -c -w /path/to/wordlist -u https://target/FUZZ -e .txt, .html, .php  -recursion -b `"MyCookie=MyAuthentData"`
```


### Intermediate
So lets say you're doing your thing and end up on a Wordpress 5.4. You notice that pages have a generic "/year/monthnum/day/hour-minute-second.html" format that got patched [here](https://core.trac.wordpress.org/changeset/47635/)
```
} elseif ( $qv['p'] ) {
	$this->is_single = true;
} elseif ( ( '' !== $qv['hour'] ) && ( '' !== $qv['minute'] ) && ( '' !== $qv['second'] ) && ( '' != $qv['year'] ) && ( '' != $qv['monthnum'] ) && ( '' != $qv['day'] ) ) {
	// If year, month, day, hour, minute, and second are set,
	// a single post is being queried.
	$this->is_single = true;
} elseif ( '' != $qv['pagename'] || ! empty( $qv['page_id'] ) ) {
	$this->is_page   = true;

```
So now you're all "well I could use Burp's Intruder, but this is gonna be slow af!", well worry not, ffuf is here for you! All you need to do is to generate wordlists matching your timeframe and run is a simple:
```
ffuf -c -w hour.txt:FHOUR -w minute.txt:FMIN -w second.txt:FSEC  -w year.txt:FYEAR -w month.txt:FMONTH -w day.txt:FDAY -u https://target/FYEAR/FMONTH/FDAY/FHOUR-FMIN-FSEC.html
```
Hope you have a small timeframe though :p


### Advanced
Some smart dude set his website to always answer with 200 OK, but to even get to that website you also need to bounce through an SSH tunnel on port 1234 that you set up due to IP whitelisting.
After checking it closer, you figure out that a query gets a reply that has "wrong params bud" in the html body when you send a wrong param/value combo and that if you spam it you'll get blacklisted. Oh you also want the real 200s to get pushed to your burp for further investigation... Well, ezpz:
```
ffuf -c -w params.txt:FPARAM -w vals.txt:FVALS -x http://127.0.0.1:1234 -replay-proxy http://burp:8080 -u https://target/ -X POST -H "Content-Type: application/json" -d '{"FPARAM": "FVALS"}' -fr "wrong params bud" -p "1.0-2.0"
```
Just have to wait for your burp to get hits now!
