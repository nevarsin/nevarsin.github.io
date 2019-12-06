---
layout: post
title:  "An old Raspberry PI as a 4G/LTE home router"
description: Every geek has at least one unused Raspberry Pi laying around, this is a tutorial to set it up as your own 4G/LTE home router. 
date:   2019-12-03 13:30:00 +01
categories: linux
author: nevarsin
---
How to (and why you should) use an old raspberry to get a full managed 4G home router (antispam included).

![]({{site.baseurl}}/images/raspberry1.jpg)
(the mighty Raspberry Pi 1 Model B)

My family's bandwidth needs are limited to web-browsing and some occasional video stream but, unfortunately, my home is located where fiber internet connection is not yet in sight. 

Luckily for me Italy is one of the countries with the cheapest mobile connections (I currently get 50GB/month for about 10€) so 4G is a viable solution.

4G routers are more expensive (starting from 80€) and less available than their xDSL counterparts so I set to find an alternative. 
Additionally: most of the software on board of routers is limited when it comes to run your stuff and, on the other hand, portable 4G routers have really crappy WiFi. 

What I needed was:
- a decent WiFi AP
- a 4G modem of sort
- an actual router

For the first one any leftover cheap wifi xDSL modem/router (you or a relative may have one left by some TLC operator) will do.

I missed the second one so I had to make <a href="https://www.amazon.it/gp/product/B01M3POL6X/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1"> a small purchase </a>. I chose this because of the price and because it was documented as linux friendly.

Lastly: I needed the intelligent packet wrangler. That's where the Raspberry comes in handy.
The absolute advantage of setting up your "raspi-router" is, as usually with Linux and FOSS, the freedom to set your custom rules incl. dns filtering.

# Connecting

![]({{site.baseurl}}/images/diagram.png)

- An ethernet cable will connect one of the ports of your router/ap to the Raspberry only ethernet port
- Of course the 4G USB modem needs to be plugged in one of the USB ports. Given the fair amount of current it will draw I would suggest, for this purpose, to use a <a href="https://www.amazon.it/Anker-Trasferimento-porte-Sottile-Adattatore/dp/B0192W3HX8/ref=sr_1_4?__mk_it_IT=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=3ON67LCLPVW14&keywords=usb+hub+alimentato&qid=1575496906&smid=A2PGPJL0BBLHLX&sprefix=usb+hub%2Caps%2C214&sr=8-4"> powered USB hub</a>.

# Router/AP

1. Configure the wifi access (SSID, WPA2/3 Key, etc). Refer to your device documentation in order to do so.
2. Set a static IP address on the device (I used 192.168.100.10/255.255.255.0)
3. Disable any DHCP server
4. Set a proper admin password (!!)

# Raspberry Pi

I assume, of course, that you already have a proper distribution onboard. Being a lifelong Debian fan I cannot recommend <a href=https://www.raspbian.org/"> Raspbian</a> enough. You can follow its <a href="https://www.raspberrypi.org/documentation/installation/installing-images/README.md"> complete install guide</a>.

Also <a href="https://www.raspberrypistarterkits.com/how-to/enable-ssh-raspberry-pi/">enable SSH access</a>

## Network interface - LAN (Eth)
The eth0 interface is, by default, set to look for a DHCP server. We need to set a static IP address in the same subnet of the Router/AP. Static network configuration is located in the DHCP client file, called ``/etc/dhcpcd.conf``

so open it up with your favourite text editor and add the following:

{% highlight bash %}

interface eth0
static ip_address=192.168.100.1/24
static domain_name_servers=8.8.8.8 8.8.4.4

{% endhighlight %}

Save and apply your new conf with 

> service dhcpcd restart


## Network interface - WAN (4G/LTE USB)

![]({{site.baseurl}}/images/usb_4g.png)

Most of the USB modems you find on the market nowadays, are (**finally**) eth network interfaces with an embbedded gateway (not just USB modems bundled with the driver hell that usually comes with it).
This means that, once you have a working SIM (disable the PIN coe first), plugging in the USB modem will most likely work out of the box.

The device is a USB multimode one so if you plug it in a Windows computer it will first appear as a CDROM device, install the drivers and then switch in "modem" mode.

With Linux you don't need to install anything beforehand and usb_modeswitch should immediately set the modem to... well... modem mode.

In my case, however, I got a kernel panic when usb_modeswitch kicked in. A little workaround was needed: I explained it here


## Firewall configuration
We need to install some packages 

> sudo apt update
>
> sudo apt install iptables-persistent

A basic iptables/netfilter setup has to be created in order to instruct our Raspi to masquerade all requests coming from LAN to WAN. This means that all connections coming from and trying to access anything not-local will be forwarded to the WAN interface. This is what a router does :)

Let's insert the key rules 

> sudo iptables -A POSTROUTING -o usb0 -j MASQUERADE
>
> sudo iptables -A FORWARD -i usb0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
>
> sudo iptables -A FORWARD -i eth0 -o usb0 -j ACCEPT

Let's then save the rules we just created 

> sudo iptables-save > /etc/iptables/rules.v4

Now they will be applied at every boot. **This is just the essential needed to share the 4G connection, make sure you set some drop default rules, especially if you get a public IP address on WAN** <a href="https://wooptoo.com/blog/quick-iptables-setup"> More info here </a>.

Lastly, we need to enable ip4_forwarding globally. This is done via:

> echo 1 > /proc/sys/net/ipv4/ip_forward

This also needs to be set at every boot so put that line in some place that will be executed on start. An old school example is ``/etc/rc.local`` or you (as you should as it is among the current best practices) can create a small systemd unit just for this purpose. 

> sudo vi /etc/systemd/system/enable.forwarding.service

insert this:

{% highlight bash %}

[Unit]
After=sshd.service

[Service]
ExecStart=/bin/echo 1 > /proc/sys/net/ipv4/ip_forward

[Install]
WantedBy=default.target

{% endhighlight %}

save and then

> sudo systemctl daemon-reload
>
> sudo systemctl enable enable.forwarding
>
> sudo systemctl start enable.forwarding

(Systemd unit files are nice and easy compared the old style init scripts, aren't they?)

### DHCP server

No router setup is complete without being in charge of LAN address assignement. Let's install a DHCP server.

> sudo apt install isc-dhcp-server

and put in a sample conf:

{% highlight bash %}

option domain-name "yourdomain.local";
option domain-name-servers 192.168.100.1, 208.67.222.222, 208.67.220.220;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style none;

subnet 192.168.100.0 netmask 255.255.255.0 {
  range 192.168.100.20 192.168.100.250;
  option routers 192.168.100.1;
  option broadcast-address 192.168.100.255;
}

{% endhighlight %}

Make sure that you don't have any other DHCP server running in your LAN and then start
> sudo service isc-dhcp-server start

You should now already be able to browse the internet from any device connected to the AP Wifi.

You probably noticed that I put the LAN address of the Raspi as the first DNS server to provision to clients. This is because we're now gonna be looking at the antispam part of the tutorial.

# PiHole

<a href="https://pi-hole.net/">![]({{site.baseurl}}/images/pihole_logo.jpg)</a>

<a href="https://pi-hole.net/">PiHole</a> is a great FOSS project that does simply one thing: all DNS requests directed to Ads domains are resolved with a bogus address. This is much more effective that browser side ad blocker extensions: the ads are not hidden/suppressed, they're simply never loaded. This will also work automatically on all your home devices.

Install it is pretty easy. There's a nice a script to run:

> curl -sSL https://install.pi-hole.net \| sudo bash



An installation wizard will run and ask you for some basic parameters

![]({{site.baseurl}}/images/pihole_inst1.jpg)

Some tips:
- when asked about which network interface select "eth0"
- choose your preferred upstream DNS (I personally dig anything but Google's)
- enable all blacklists
- consider whether to enable the dashboard: while it is cool, it will end up doing a lot of writings to your Raspi's SD and you will just look at it once or twice.

Ok now you also have a fancy, adfree, DNS server.

# Conclusion

Done! you now run a fancy home made router you have full power on. Some other things you may set on it:
- Attach a USB drive and run a <a href="https://transmissionbt.com/">Trasmission server</a>
- Configure a VPN provider on it and make your whole home network run through it

I will be covering some more related topics in the future. In the meantime thanks for reading and feel free to share.

