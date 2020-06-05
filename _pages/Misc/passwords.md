---
title: Passwords
---

## What is this about?
Yep, we're going there... Passwords, how do they work?

A password is an ordered charset that you give to something and it lets you go to the next step of the automation if it matches the one it has, with a couple of extra rules around it. To attack a password, you either attack the *before* i.e clear text input, or the *after* i.e the hash itself. You can also attack the *how* it's hashed, but that's another story. 

### The usually debated things
* Length: The longer the password, the more nested loops an attacker will need to go through to get the complete set available.
> a, aa, aaa, aaaa, aaaaa, ...
* Charset: The bigger the charset, the more characters an attacker will have to go through before the complete set is generated.
> a, b, c, d, e, ...
* Distribution: How many times the same character will appear on a password
> With [a-b]: aaaa -> a(4).b(0), aaab  -> a(3).b(1) aabb  -> a(2).b(2), ...
* Complexity: The more combinations there are, the more complex the password will be.
> A password with one character in length but with a charset containing 10 characters is basically the same as a password of length 10 but with a charset containing only one character... You'll need 10 hits to cover the entire set for both *given that different params are known*. But hey, just Google the math if you want more info...

### The usually debated things *IRL*
* Length: The maximum length of a password is defined by how it will end up stored. We're not going to cover hash collisions in detail here, but know that your password will end up in a defined set that has a limited amount of space available and that it is statistically possible that 2 password, which come from a set that is *bigger* than where they end up in, may have the same hashed value. How often this happens is defined by the math behind the hashing function used. Check [this](https://en.wikipedia.org/wiki/Birthday_problem) and [that](https://shattered.io/) out for more explanations and examples. 
TL;DR: If your password is long enough it might have the same hashed value as "a" and the computer checking if the hashed value matches the one it has won't know the difference, but this happening is highly unlikely due to many factors starting with max password length...

* Charset: To keep this simple, the charset we'll use is defined by anything between 0x20 (space) and 0x7E (\~) on the ASCII table. This means that in total there are 95 different characters you can work with to create your password. Once you add the hashing part though, all you're doing is giving the hash function a chain of values to work with either it's "a" or "$" it doesn't matter.

* Distribution: Since we're working with a charset that's n characters long, we can generate a password of length n with all unique characters, which means that the n+1 character will 100% be a repeated character. The more diverse the characters you use, the bigger the charset that will be needed to crack your password.

* Complexity: [Cognitive bias](https://en.wikipedia.org/wiki/List_of_cognitive_biases) is the problem here. I'll just go through 2 examples, you'll get the point.
  * Longer passwords are better, so we should write sentences to make it really long
     * The charset here is [words in a given language] which means you have quite a big charset, but your password will only be a couple of words long because the *per letter* length you're looking at is longer. If the charset is known by the attacker and/or tested just because, this password/passphrase will actually be weaker than a shorter fully random password.
  * Using a big variety of characters is better, so there should be x amount of y *type of* characters to make it harder to *guess*.
     * The characters used are still a subset of those 95 in the ASCII table though. Sure for a pure bruteforce with all of them, you're adding time, but for a rainbow table attack, well lets just say anything under 8 characters long and unsalted is going to be precompiled and [instantly cracked with stuff found online](http://project-rainbowcrack.com/table.htm). Then it's just a question of throwing cash at the storage problem if you need to generate bigger tables...

### The basic methods used to crack passwords
Yes, you can do other things like phish for passwords, keylogging, record the user's screen, ..., but we're not talking about this here since nothing on this page will matter if you do...

* bruteforce: Testing every combination of characters possible until successfully entering the correct password
  * Pros: You will find the password, 100% guaranteed
  * Cons: The longer the password and the bigger the charset, the longer you will take to crack it
* Dictionary attack: A language is a limited set of words that are put together, therefore you test the words and not the characters
  * Pros: *I like mashed potatoes* becomes a lot quicker to crack if you're looking at words instead of letters
  * Cons: Not every character combination makes a "word", which means you're trading depth for breadth
* Rainbow tables: If you generate every combination of characters in your charset, you *know* every password up to a given length. Then if you run those those combinations through the same algorithm that was used on whichever password you're trying to crack and store the before and after, you can *instantly* get the password of any given hashed password in your given set.
  * Pros: Time is no longer an issue for your pure bruteforce attack
  * Cons: You need to store those rainbow tables somewhere and the more you have, the more space it takes. i.e. you're literally throwing cash at *time*
* Rulesets: Humans have a tendency to use patterns when generating passwords, if you generate rules to match those patterns and sort them by most to least common, you will statistically speaking improve your crack rate.
  * Pros: You will quickly crack passwords that match the rulesets
  * Cons: You will never find the passwords that don't match the rulesets

Note that you can also combine methods and go for a hybrid mode, which means that for example [leetspeak](https://en.wikipedia.org/wiki/Leet) will be tested...

### What is all of this *need x type of y* about?
This is about spreading the charset out so that in a bruteforce scenario it'll take more time and in a dictionary/rainbow attack scenario whatever the password is, it's supposedly not in a pre-compiled dictionary. Now the charset you're working with changes depending on your alphabet/language/..., but we're following the ASCII table here.

Couple of common splits in the available charset:

* \[a-z\]: This charset contains only lowercases letters, the charset is of size 26. 
> potato
* \[A-Z\]: This charset contains uppercase letters, the charset is of size 26.
> POTATO
* \[0-9\]: This charset contains numbers, the charset is of size 10
> 7470
* The "special character" charset contains whatever doesn't match the previous splits, the charset is of size 32
> !%$&#

Now if you mix them up, you get bigger and bigger charsets, for example
* \[a-z\] + \[A-Z\]: This charset contains upper and lowercase letters, the charset is of size 52.
> Potato
* \[0-9\] + \[A-Z\]: This charset contains uppercase letters and numbers, the charset is of size 36.
> POT00000000

But this split in character type is only done because *humans* write passwords and security people are trying to make them harder to crack in a simple and easy way...

### Other common things around passwords

#### Attacker's side
* Credential stuffing: Trying out passwords that the same person has already used, because you never know they might reuse them...
  * Pros: You might enter the correct password on your first try
  * Cons: It's probably not going to happen every time ;)
* Password spraying: Instead of trying to find a password for an account, the most common passwords are tested on as many accounts as possible.
  * Pros: You might quickly find a low hanging fruit
  * Cons: It's probably not going to happen either ;)
* Relaying hashes: Why crack the password when you can do the same thing without having to crack it?
  * Pros: You can ignore everything here
  * Cons: Your access will often be limited

#### Defender's side
* Password expiration: 
  * Pros: If it takes more time to crack a password than the timeframe it's usable in, then cracking it will not lead to a breach.
  * Cons: Users will probably partially reuse passwords and change only one character every time they need to update it.
* Salting passwords: 
  * Pros: Rainbow tables need to be regenerated per salted hash value
  * Cons: From a password security's only point of view, none!
* Password managers:
  * Pros: You can have one password to rule them all, which means you can generate randomized passwords, maximize the charset and length used and have a unique password per service. 
  * Cons: There only is one password to crack to access everything, you're also relying on the security of your password vault.

#### General stuff
* Topography: In general, humans suck at remembering what is perceived as a random list of characters so they'll try to build something that is easy to remember. Why? Well, lets say you have a password for your work email, your private email, your computer, your bank account, that website where you bought shoes on once but might buy more on so you created an account, your phone, that zip file with your personal info, ... If you can't remember a single password built with, lets say, 128 random characters how are you going to remember all of those passwords?

Which means that at the end of the day it's all about *security vs ease of use* again!


## Building the policy
You have something to lock-down and need to define how the password to access it will be built? This is usually the "12 letters, numbers, special chars, 3 Cyrillic letter equally spaced in the password, etc" thing that is usually done... 

### So why are people fighting over this stuff?
Password complexity and understanding *how* the cracking is done and *why* the solution they have is not perfect due to attackers and defenders playing the cat & mouse game. It's also a matter of resources needed to crack a password before it becomes obsolete.

### So what are you proposing?
Simple, generation and storage should not be done by humans due to biases and limitations in our brain capacity, so use a secure password manager combined with a [Yubikey](https://www.yubico.com/passwordmanager/) or something and make everything as complex as your system will allow.

### I can't do that because of reasons
¯\\_(ツ)_/¯


## Breaking the policy
Lets jump on the other side of the fence now and go through a parallelizable set of steps.

### Step 1
Find the charset, min and max length, hashing algorithm, ... , that are used and write the matching ruleset. This is so you won't be losing time on things that will fail anyway...

### Step 2
Run through your Rainbow tables. Download a list or make it yourself and you will get insta-cracks if there are passwords that match that charset...

### Step 3
You now know that the passwords left are not in your rainbow table due to being bigger, smaller or that there are characters that you haven't compiled and tested. So now you should be running through leaked passwords and/or checking for password reuse.

### Step 4
Take a step back and think about what you want. Is it access to a system through an admin's account? 
Crack all the passwords used? Get new passwords for your Step 3? Now is the time to properly define what you want and check what you have. i.e Why spend some time cracking the password of the dude that has no access rights if you already have the password of a domain admin?

### Step 5
You are entering ruleset zone here. Recheck the password restrictions that are applied to your target, find a couple of [pre-defined rulesets](https://github.com/praetorian-code/Hob0Rules) modify and run those. Ofc remove anything you already went through in the previous steps... Remember that you can reuse the passwords you've already cracked for that given target to create new wordlists and rulesets on the fly. Don't forget Hybrid mode.

### Step 6
Whatever password hasn't been cracked by now, well good luck with your pure bruteforce if you really want to crack them. In the meantime, you should be looking for another access point...


## TL;DR

It's always about security vs ease of use combined with tolerance to how much time you want to spend on each side of the fence.

If you can protect access to something until that something becomes irrelevant, you're good.

Whatever policy you put together, if *humans* have to generate AND remember the password, you're going to end up with a bundle of shit passwords that can be cracked easily and a single one is all an attacker needs to wreck your stuff. 

The simple solution is to stop the dick measuring contest, enforce a length of whatever arbitrary high number/maximum amount of characters available and a charset containing *every* character available. **Then enforce that policy from day 1, teach your workforce how to use password managers and config them to only generate that type of password**. Your DB gets breached? Good luck cracking fully random 256 characters long passwords to escalate with...

By doing that, you've just reduced the amount of passwords that need to be remembered to your master password + whatever steps before being able to read it (e.g. computer password). This makes it a lot easier to remember and allows you to build a single much more complex password.

You can also rotate ALL the passwords whenever you feel like it, because using them is just a copy paste away and you don't even need to know what the password is! This also takes care of those "Welcome2020" that become "Welcome2021" when it needs to be updated...

Yes do use MFAs...

Finally, whatever hardcore passwords policy you set up, it's only a really small part of the attack surface and it can always be bypassed.

Note: A certificate is a basically a very long password that you can use to identify something. You might want to look into that for the easiest increase in security possible on your network, web, email, source code, physical access, ... security you need!