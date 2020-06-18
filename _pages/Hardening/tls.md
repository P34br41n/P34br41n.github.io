---
title: TLS
---

## What is this about?
Upping your security game through better web security

This won't be a monologue about RSA vs ECDSA vs Ed25519, or why the only answer to *but my grandma uses [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape) and I need that [S-HTTP](https://www.ietf.org/rfc/rfc2660.txt) option for my custom things at home*, should be *lol, fuck off*.

Fun story, I had to pentest a *we forgot about this for 20 years, checkout the spiderwebs on it!* website serving S-HTTP in another life, which lead me to recoding a basic browser that would talk with it... Fun times with security by obsolescence! :p

## TLS configuration generator
Use this to generate your config file for whichever software you use to serve your website.

The why is here:

[https://wiki.mozilla.org/Security/Server_Side_TLS](https://wiki.mozilla.org/Security/Server_Side_TLS)

The how is there:

[https://ssl-config.mozilla.org/](https://ssl-config.mozilla.org/)

## Testing your configuration
If you're thinking "nah I'm good no need to do this", just run a simple [testssl.sh](https://testssl.sh/) on your website and start from there ;)

## Client Cert Authentication
A somewhat generic docker-compose.yml that I scraped together and tend to re-use as a skeleton before adding specifics and hardening it. Replace whatever you need to. This includes HAProxy and 2 httpd servers to test it out. You'll also need a server cert/user cert combo. 

Yes, this is basically MFA with the *what you have* part being your cert and if you have half a braincell working, you can deploy it everywhere and run an alert back to your SIEM when someone loads the page without a cert. Because hey, everyone legit is supposed to have one...

### docker-compose.yml
```
version: '3'

networks:
  frontend:
  backend:
    internal: true

services:
  app1:
    image: httpd
    container_name: app1
    restart: always
    ports:
      - "8080:80"
    networks:
      - backend

  app2:
    image: httpd
    container_name: app2
    restart: always
    ports:
      - "8081:80"
    volumes:
      - ./html:/usr/local/apache2/htdocs
    networks:
      - backend

  haproxy:
    image: haproxy
    ports:
      - 80:80
      - 443:443
    container_name: 'haproxy'
    volumes:
      - "./haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg"
      - "./ssl:/etc/ssl"
    networks:
      - frontend
      - backend
```
You'll need a folder *html* with an *index.html* file in it, to check which app you're hitting when you have a cert/don't have a cert. It's either going to be *it works!* or whatever you put in the index.html.

### haproxy.cfg
```
global
    ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
    ssl-default-bind-options no-sslv3 no-tlsv10 no-tlsv11 no-tlsv12 no-tls-tickets

    ssl-default-server-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
    ssl-default-server-options no-sslv3 no-tlsv10 no-tlsv11 no-tlsv12 no-tls-tickets

defaults  
    log global
    mode http
    timeout connect 5000  
    timeout client 50000
    timeout server 50000

frontend http
    bind *:80
    redirect scheme https code 301 if !{ ssl_fc }

frontend https
    bind :443 ssl crt /etc/ssl/combo.pem verify optional crt-ignore-err all crl-file /etc/ssl/ca-crl.pem
    option httpclose
    option forwardfor
    use_backend app2 unless { ssl_c_verify 0 }
    use_backend app1 if { ssl_fc_has_crt }

    default_backend app1

backend app1
    option httpchk GET /
    server srv1 app1:80

backend app2
    option httpchk GET /
    server srv2 app2:80
```
ca_crl.pem is your revocation list

combo.pem is built with
```
cat public.pem private.pem chain.pem > combo.pem
```
Drop them both in an *ssl* folder.

Now run the following line from the base folder
```
docker-compose up
```
Try with and without your cert, don't forget about the cache though ;)
