---
layout: post
title:  "How to set up a Wi-Fi hotspost on Kali linux"
date:   2017-01-02 08:00:00
categories: Kali Wi-Fi
tags:
- Kali
- Wi-Fi
- Hotspot
---

## Acknoledgment
I pretty much used (this)[https://www.psattack.com/articles/20160410/setting-up-a-wireless-access-point-in-kali/] guide, but I 
slighty modified the config files to make the configuration easier.

## Disable NetworkManager
First thing to do is to disable our Wi-Fi interface from Network-Manager:
We can do that by adding the access point mac address to /etc/NetworkManager/NetworkManager.conf:

```Bash
[keyfile]
unmanaged-devices=mac:xx:xx:xx:xx:xx:xx
```
and then we can restart the NetworkManager with `service NetworkManager restart`

## Hostapd
Hostapd is an access point software you can use to turn your Wi-Fi adapter to an hotspot.
After installing Hostapd with apt-get install hostapd we must create a config file with our parameters:

```Bash
interface=wlan0
driver=nl80211
ssid=basic
channel=1
auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
wpa_pairwise=CCMP
wpa_passphrase=testpw12345
```

Where the interface is my wi-fi interface (you can see what is yours using `iwconfig`), the driver is a generic driver which works with most wi-fi cards, and finally I choose as my Wi-Fi ssid "basic"
and aWPA2 encryption with testpw12345 as a password.

You can do all sorts of things with hostapd settings, as shown in "site" and "site"

## Dnsmasq
I do not changed anything from the original (guide)[https://www.psattack.com/articles/20160410/setting-up-a-wireless-access-point-in-kali/]  
apart not using a dns_entries file because I didn't need one.
So after installing dnsmasq (apt-get install dnsmasq) I created the config file on /etc/dnsmasq.d/hotspot.conf:

```Bash
interface=wlan0
dhcp-range=10.0.0.10,10.0.0.100,8h
dhcp-option=3,10.0.0.1
dhcp-option=6,10.0.0.1
server=8.8.8.8
log-queries
log-dhcp
```

## Automated script
I created an automated script which starts all the services I need and wait for a Ctrl-C to close the hotspot.
You can find the source code, and the config files here.
If you prefer to set up the hotspot manually here are the programs:

## TroubleShooting
This is the most commond errors that happened during my tests:

