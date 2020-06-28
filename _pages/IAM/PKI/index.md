---
title: PKI
---

## What is this about?
Managing user keys through a centralized service, which makes it easier for everyone!

## Step 0
Read [this wiki page](https://tools.ietf.org/html/rfc2459) and [this RFC](https://tools.ietf.org/html/rfc5280).

## The basics
Looking for that extra security in your network? Need to identify users in a way that's a bit more precise than just a login/password combo? Want some MFA but can't figure out how or don't have the budget? Well welcome to the certificate world!

### What's a certificate?
To keep this simple, a certificate is like a passport with some specific info on it that allows everyone to identify and be identified as something known. You also get to add info to identify the owner on it like a name, country, organization, email address, IP, ..., and at the same time, you also get a public/private key combo (so basically a very long password) to actually do stuff with it and prove that you are who you say you are the *electronic* way! 

Think of a cert as your standard asymmetrical key combo on steroids!

### CA structure
This is a simple version of what you should be doing when generating CAs for your environment. The relationship tree is fairly simple
```
                             RootCA
             SubCA __________/ | \__________ SubCA
              |                |              / \
              |                |            /     \
              |              SubCA        /         \ 
            SubCA             /|\      server     server
             / \            /  |  \
           ... SubCA      /    |    \
                ...     user  user  user
```
Your RootCA has 3 SubCAs that are used, from left to right, to generate more SubCAs, user certificates and server certificates. What are those you ask?

#### RootCA
This will be where everything comes from and what signs every other thing you will generate. You'll need to deploy the public key to every endpoint and server to be sure it's recognized by everyone and then you're good to go!

RootCAs are there so that you can generate CAs to actually do stuff with and revoke them if you ever need to. That's it, **don't do anything else with it**, don't deploy the private key anywhere and don't let anyone access it at any time. If this certificate leaks you'll need to delete all the CAs you've ever deployed, create a new one and redeploy everything, because having access to the RootCA means that you can generate anything you want with it...

#### SubCA
A Subordinate CA is basically like a RootCA that can be revoked. It can be used to do the same things, or not, depending on how you set it up. This is the thing you want to actively use and deploy to your environment and if you ever have a problem, revoke it and create a new one... 

Now this is the thing, if you revoke it then anything under it in the tree will be unusable if that's the only entity that signed it. It's built this way so that if the private key leaks and is used to sign stuff then it won't be recognized as valid if you revoked it, well that's if you did everything else correctly.

You'll still need to send that revoking info to everything in your environment and possibly outside of it when it happens.

#### User/Server
These certs are at the end of the line and are actively used daily for more or less everything, once you have everything setup of course... For example, it can be used by a user to identify himself, by a machine to connect to Wi-Fi, by a server for the whole TLS thing and whatever else you can come up with.

Every entity should have a unique cert filled up with as many attributes as you feel are necessary. This way you can identify the entity and do stuff depending on the info that's there!

### Certificate Revocation List
When you start handing out certificates, sometimes you might want to revoke some of them because the private key has leaked, the owner left the company, the server got deleted, it was for a test or whatever else you can think of... 

Because of that and just to be sure that the revoked certificate cannot be used anymore you write down specific info about it in a list, push that list to every system the cert can possibly be used on, and this basically adds a *this certificate isn't valid anymore so don't accept it* line everywhere.

### Online Certificate Status Protocol
The [OCSP](https://tools.ietf.org/html/rfc6960) is basically a CRL that you can query from to know if a certificate is still valid or not. This allows you to revoke a certificate and not have to wait for the CRL propagation to happen to deny access. It still is a good idea to push CRLs as a rollback method though, just in case the OCSP isn't accessible for whatever reason.

If you don't have the need for it now, it's cool, you can always automate the push of the CRL and add this later on.

### Securely storing the RootCA
Follow these simple steps!

* Get a couple of [IronKeys](https://en.wikipedia.org/wiki/IronKey) or whatever secure and encrypted storage medium you want.
* Copy the private key from your RootCA and erase it from your server/pki.
* Paste the RootCA's private key on the secure medium you got.
* Store those secure mediums in different places, possibly locked away.
* When needed, get one of them from a vault and restore the private key to the server/pki.
* Do your stuff.
* When done, delete the private key from the server/pki and put the medium back in the vault.

Note: backups should be temporarily turned off when the RootCA is on the server since you don't want it to be accessible somewhere other than on the secure mediums. You could also do everything from the medium depending on your setup.

## What is this good for?
Absolutely everything! If you need to identify users, machines, services, ..., then a certificate can be used to increase the general security level. Here are a couple of examples that you can implement!

### Wi-Fi - EAP-TLS
Got Wi-Fi and need to secure it? Then you can setup [EAP-TLS](https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol#EAP_Transport_Layer_Security_(EAP-TLS)) and if you configure everything right, then you can forget about someone breaching your Wi-Fi without a valid certificate. Don't forget about hardening whatever connects to your network and remember that certificates can be pulled from machines though...

### Network access control
Your network is a flat mess and you'd like to throw in some complexity and access control? Then you might want to look at [NACs](https://en.wikipedia.org/wiki/Network_Access_Control) and set one up. You can then match on certificates and redirect whatever is using them to whatever VLAN you want by checking attributes or something ;)

### Email - S/MIME
Need that extra level of email security? Need to identify your users to be sure *Bossman@1337Corp.pwn* is the real one or just someone faking it? Want to encrypt messages to keep things a secret? Then add [S/MIME](https://en.wikipedia.org/wiki/S/MIME) to your emails!

### Code signing
You need to secure your dev chain and want to be sure your code isn't being tampered with? You want to clearly identify the dev in your version control by marking the underlying code? Well [code signing](https://en.wikipedia.org/wiki/Code_signing) is there for you!

### Web client authentication
Want an extra security step that's totally transparent to the user? Want to lock down your internal websites and instantly have a flag raised when someone unauthorized does anything on them? Simple! Checkout [Client authentication](https://en.wikipedia.org/wiki/Client_certificate)!

### Smart card
You need to setup some access control to a building? Want some MFA to login to computers and come in to the office and emails? Certificates do it [all](https://en.wikipedia.org/wiki/Smart_card#Identification)!

### SSH authentication
Can't handle the whole SSH key stuff going on and have no time to add and remove keys from servers depending on whatever weird access rights users need? You just need an [alternative branch](https://roumenpetrov.info/secsh/index.html) of SSH that supports X.509!

### IPSec/VPN
The whole login and password combo is too cumbersome? You need that little extra thing to secure access to your network? Well check [this](https://en.wikipedia.org/wiki/Virtual_private_network#Authentication) and then [that](https://openvpn.net/community-resources/setting-up-your-own-certificate-authority-ca/) for an example!

## Extra things and thingies

### CA/cert validity
A simple time percentage baseline to follow would be 
* RootCA: 100%
* SubCA: 50%
* Cert: 10%

Which means that if you go for a RootCA that has a 10 year validity period, then your subCA should be set to 5 years and your users should have a cert valid 1 year. But hey if you want to run the same RootCA for 100 years go for it! Your setup will probably be obsolete by then and your stuff will probably be bruteforced instantly, but whatever needs to be done to work less right?

### CRL stuff
#### Delta
Just forget about this if you're not in specific environments where everything supports it, just go for OCSP if your CRL gets too large and unmanageable, or replace the SubCA every so often to dump everything just in case you forgot to revoke some certs...

#### Validity
This is the thing, CRLs also have a validity period that makes them unusable after the time's up. So what you usually do is set the CRL to be auto generated and pushed a couple of times every day and have the validity set to a week. This allows for plenty of redundancy and revocation updates and if something crashes the next CRL update will probably pick it up. Worst case scenario you have a week to fix you stuff before everything goes to shit.

If you don't do this and you go for a CRL that's valid for a week and you publish it every week, then during an entire week revoked certificates will still be accepted. Even worse, if your stuff crashes and the new CRL isn't received on time then, depending on the system, users won't be able to do anything that requires certificate authentication until the new one is pushed because the CRL isn't valid and therefore every cert will be refused.

## IRL
```
Coming soon!
```
[EJBCA](./ejbca)
