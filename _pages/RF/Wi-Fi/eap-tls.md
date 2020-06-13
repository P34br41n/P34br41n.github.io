---
title: EAP-TLS
---

## What is this about?
A specific version of certificate protected Wi-Fi access. This is also usually combined with a [RADIUS](https://en.wikipedia.org/wiki/RADIUS) server to check the validity of the cert and a [NAC](https://en.wikipedia.org/wiki/Network_Access_Control) to do the network/VLAN redirection part...

### Step 0
Read [this](https://tools.ietf.org/html/rfc5216) and [that](https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol#EAP_Transport_Layer_Security_(EAP-TLS))

### OK, so how do I get in?
If everything is setup correctly and you don't have a valid certificate, well you better find one because it ain't happening...

### Surely there must be a way in!
Yep, but it's through exploiting misconfiguration of clients and/or escalating your access until you can get a valid certificate.

### Extremely simplified version of how stuff works
* The client connects to the AP but doesn't have any network access
* The AP sends an auth request to the client
* The client sends his ID which is passed on to the RADIUS
* The RADIUS responds with an OK/NOT OK
* They both go through the EAP-Requests/Response steps of the TLS tunneling part
* Bob's your uncle

If you want/need more details just read the RFC in *Step 0* copy pasting is a waste of time/space...

### Client misconfiguration
Endpoints that aren't configured to validate and/or don't warn of new/weird CAs or even if the user clicks OK to authenticate on a CA that isn't the expected one will still continue the steps and connect to a server. The only thing that will block them is that the AP that they're connecting on will deny access. So how do you get a user to connect to *your* network with any certificate? Just force your RADIUS server to always respond with an "OK, you can continue your cert seems legit"...

#### Wut?
The server has a CA that's signed by whatever entity, this is usually a SubCA dedicated to servers or even one dedicated to just this task, which most of the time if you go back up the CA chain you end up with a RootCA from an internal PKI or something. 

Now what happens during the whole exchange to connect is that the server will send its identity to the client through its public key and the client will check that it's the correct one. 
The server on the other hand will do the same with whatever comes from the AP, because at the end of the day you're checking if a client can connect to the network with a valid certificate... Which means that the AP has *access control* and that's what this is all about.

#### Yeah ok, so now what?
When an endpoint connects to an AP, and the AP's cert isn't valid there are 3 possibilities
* Your endpoint's security is so bad it won't even bother telling the user there's something weird going on and will follow through
* You'll get that "warning authenticating to a weird network with a weird cert, please check that everything is legit before connecting" message, but who actually checks that... Right?
* The endpoint is properly hardened and will just refuse to connect and you'll get a ticket saying something like "my mofo Internet is busted again, plz fix ASAFP.

### So you're saying there's a chance?
Well if restriction to specific CAs for server/AP/access isn't setup correctly and/or the user proceeds anyway with an untrusted certificate, then yes the game is on!

### The battle plan
#### The uncomplicated part
Follow the bullet points and don't fuck it up!

#### The AP setup
Get the hostapd-2.6 source code [here](https://w1.fi/releases/hostapd-2.6.tar.gz) and apply the [hostapd-wpe](https://github.com/opensecurityresearch/hostapd-wpe) patch. You possibly don't need to, but hey this is my setup for other stuff too, it will make your life easier and cherry-picking is a thing! ¯\\_(ツ)_/¯

Your */hostapd/.config* file should look like this
```
CONFIG_DRIVER_HOSTAP=y
CONFIG_DRIVER_WIRED=y
CONFIG_DRIVER_NL80211=y
CONFIG_LIBNL32=y
CONFIG_IAPP=y
CONFIG_RSN_PREAUTH=y
CONFIG_PEERKEY=y
CONFIG_IEEE80211W=y
CONFIG_EAP=y
CONFIG_EAP_TLS=y
CONFIG_EAP_IKEV2=y
CONFIG_EAP_TNC=y
CONFIG_EAP_EKE=y
CONFIG_PKCS12=y
CONFIG_RADIUS_SERVER=y
CONFIG_IPV6=y
CONFIG_TLS=openssl
CONFIG_TLSV11=y
CONFIG_TLSV12=y
```
Your */hostapd/hostapd-wpe.conf* like this
```
interface=wlan0
ssid=hostapd-wpe
auth_algs=1
wpa=2
ieee8021x=1
wpa_key_mgmt=WPA-EAP
eapol_version=2
eap_server=1

eap_user_file=hostapd-wpe.eap_user
ca_cert=../../hostapd-wpe/certs/ca.pem
server_cert=../../hostapd-wpe/certs/server.pem
private_key=../../hostapd-wpe/certs/server.pem
private_key_passwd=whatever
dh_file=../../hostapd-wpe/certs/dh

hw_mode=g
channel=1
logger_syslog=-1
logger_syslog_level=2
logger_stdout=-1
logger_stdout_level=2
ctrl_interface=/var/run/hostapd
ctrl_interface_group=0
beacon_int=100
dtim_period=2
max_num_sta=255
rts_threshold=2347
fragm_threshold=2346
macaddr_acl=0
ignore_broadcast_ssid=0
wmm_enabled=1
wmm_ac_bk_cwmin=4
wmm_ac_bk_cwmax=10
wmm_ac_bk_aifs=7
wmm_ac_bk_txop_limit=0
wmm_ac_bk_acm=0
wmm_ac_be_aifs=3
wmm_ac_be_cwmin=4
wmm_ac_be_cwmax=10
wmm_ac_be_txop_limit=0
wmm_ac_be_acm=0
wmm_ac_vi_aifs=2
wmm_ac_vi_cwmin=3
wmm_ac_vi_cwmax=4
wmm_ac_vi_txop_limit=94
wmm_ac_vi_acm=0
wmm_ac_vo_aifs=2
wmm_ac_vo_cwmin=2
wmm_ac_vo_cwmax=3
wmm_ac_vo_txop_limit=47
wmm_ac_vo_acm=0
eapol_key_index_workaround=0
own_ip_addr=127.0.0.1
```
You might want to change the interface, SSID, channel, ... The certs that come with hostapd-wpe will work, but you probably want to create new ones to match as much as possible the ones that are used, so that if someone needs to click an OK button, he will... I mean who goes and check the validity of self-signed certs anyway, you only check the name right?

Finally your */hostapd/hostapd-wpe.eap_user*
```
# Wildcard for all identities
*	TLS
```
Yes there are a couple of extra options in the config files that you don't need, but whatever ;)

Now, open */src/eap_server/eap_server_tls.c* and change a *1* to a *0* in the eap_tls_init function
```
static void * eap_tls_init(struct eap_sm *sm)
{
	struct eap_tls_data *data;

	data = os_zalloc(sizeof(*data));
	if (data == NULL)
		return NULL;
	data->state = START;

	if (eap_server_tls_ssl_init(sm, &data->ssl, 1, EAP_TYPE_TLS)) { <--- this 1 becomes a 0
		wpa_printf(MSG_INFO, "EAP-TLS: Failed to initialize SSL.");
		eap_tls_reset(sm, data);
		return NULL;
	}

	data->eap_type = EAP_TYPE_TLS;

	data->phase2 = sm->init_phase2;

	return data;
}
```
Go back to */hostapd/* and build it
```bash
#Just in case you forgot how to do that, lol :D
make
```
You might need to install *libnfnetlink-dev*, *pkg-config*, *libnl-3-dev*, *libnl-genl-3-dev* and possibly *docbook-utils* to make it work.

To start the hotspot all you need now is to run a simple now
```
sudo ./hostapd-wpe hostapd-wpe.conf -dd
```

#### The Network stuff
There are many ways to setup a DHCP server and route everything correctly, this is just a minimal version so do as you please...

Your */etc/dnsmasq.conf* should look like this
```
interface=wlan0
dhcp-range=10.0.0.10,10.0.0.15,12h
```
Yeah anything else is default or extra stuff, but this is a minimal version remember...

Now, run the following in your term. Here eth0 would be my actual network access and wlan0 the AP
```
ip link set eth0 down
ip addr flush dev eth0
ip link set eth0 up
ip addr add 10.0.0.1/24 dev wlan0
killall dnsmasq
dnsmasq
```
You're all done!

#### The fuck did I just do?
You created an AP, that's a totally legit AP, and that you can now setup so that everyone who tries to connect, with whatever certificate they have, does... What? This is what we want to do, it isn't weird or anything, 100% legit and certified by a trained professional stuff right here!

After that you setup a DHCP sever so that clients won't drop out of your network. All you need now is a captive-portal to phish for stuff, or a couple of iptables POSTROUTING/MASQUERADE/FORWARD rules to NAT everything to whatever network you're on and possibly to Internet.

### The Setup you should go for
Build and run everything on a raspberrypi, plug it to a powerbank and walk around. SSH to your pi with your phone and wait for connections/creds/whatever you're going for... It's as simple as that.
