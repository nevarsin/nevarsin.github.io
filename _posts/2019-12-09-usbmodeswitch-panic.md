---
layout: post
title:  "Raspberry, USB Modeswitch and Kernel panic"
description: USB_modeswitch, on Raspbian, is a bless. Sometimes, however, things can "panic". This is how I fixed it.
date:   2020-01-12 18:30:00 +01
categories: linux
author: nevarsin
published: true 
---
|![]({{site.baseurl}}/images/usbmodeswitch_header.png)|
|:--:| 
| *the default usbmodeswitch conf file* |

# Introduction

This article is more of a spin-off of <a href="/raspberry-router/">my tutorial on how to turn an old Raspberry Pi on a smart and free home router</a>. I will describe here what kernel panic issue I had and how I fixed it. Hope it can be useful for other purposes and with different hw.

# What is USB_Modeswitch and why do we need it?

USB Modeswitch is a simple but effective tool that, since 2010, helps Linux user/admins dealing with those weird hybrid usb dongles.


Let me explain what I mean by that:
USB dongles come in a variety of forms and they are typically single purpose (a modem, device adapter, network interface, etc). What makes them weird is that, because of Windows and its extremely lacking set of device drivers, vendors had to find a way to supply said drivers in a plug&play fashion.

This means that, when you plug one of these devices for the first time, it will
- tell the OS "I'm a CDROM! Here's the exe setup you wanna autorun!"
- install the actual driver
- switch to its actual purpose 
- start working

Linux, on the other hand, comes with a whole load of device drivers included in the kernel. So no need for that extra step.

So every time you plugged in a bt/eth/wifi/whatever dongle in your Linux workstation and saw it working right away, you have to thank usb_modeswitch (and the kernel devs, of course)
 
# What went slightly wrong

|![]({{site.baseurl}}/images/usbmodeswitch_kernelpanic.jpg)|
|:--:| 
| *my face during the whole troubleshooting* |

During my <a href="/raspberry-router/">home router installation</a>, however, the following happened:

1. Plugged in the Alcatel IK40V LTE USB dongle
2. Saw the kernel reacting
3. Saw the kernel switching mode
4. Kernel panic :(

And this, unfortunately, happened even by plugging the dongle in before booting.

# How I fixed it

Turns out that the prompt switch from CDROM mode to USB mode didn't let the kernel load the appropriate modules. The key to avoid this was doing things... slowly :) by decoupling the device set in as a CDROM device and the subsequent switch to ETH mode.

## 1 - Disable automatic USBModeSwitch

Edit ``/etc/usb_modeswitch.conf`` and set

> DisableSwitching=1

Save and close the editor.

## 2 - Get the device Vendor and Product code

Type 

> lsusb

and have a look at the following:

![]({{site.baseurl}}/images/usbmodeswitch_lsusb.png)

What you see there is the Vendor code and Product code that the kernel use to uniquely identify a device.

In my case **1bbb** is Alcatel and **0195** is the IK40V dongle.

## 3 - Fetch a sample conf 

Let's grab usb_modeswitch whole conf template library that is, on Raspbian, located at ``/usr/share/usb_modeswitch/configPack.tar.gz``

{% highlight bash %}

mkdir conftemplates && cd conftemplates
cp /usr/share/usb_modeswitch/configPack.tar.gz .
tar -zxf configPack.tar.gz

{% endhighlight %}

You will get a whole bunch of VENDOR:PRODUCT files. So let find out which file matches our device by grepping the product and vendor codes:

{% highlight bash %}

root@raspberrypi:~/conftemplates/grep "0195" * | grep "1bbb"
1bbb:f000:3:TargetProductList="0000,0017,00b7,011e,0191,0195"

{% endhighlight %}

Got it!

## 4 - Set up our own switch

Using the content of the file let's create or own conf. In my case: 
{% highlight bash %}

# Alcatel X200/X200L/X060S/L100V, Archos G9 3G Key
TargetVendor=0x1bbb
TargetProductList="0000,0017,00b7,011e,0191,0195"
StandardEject=1

{% endhighlight %}

(StandardEject means that the device will be 'ejected' while in CDROM state in the default way. I had less trouble this way)

and save it to ``~/usb_mydeviceswitch.conf``

## 5 - Test it

1. Remove your dongle (if inserted)
2. Plug it back in
3. Verify that it didn't switch (depends on your dongle, in my case no new network interface)
4. Run ``usb_modeswitch usb_mydeviceswitch.conf``

Success! You should now have your dongle working as expected.

{% highlight bash %}

root@raspberrypi:~# ip link list
[...]
3: usb0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 1000
    link/ether 86:55:9b:fa:69:c7 brd ff:ff:ff:ff:ff:ff

{% endhighlight %}

## 6 - Set this to run at boot

Final touch: run this at boot via /etc/rc.local or a new systemd service. Have a look at <a href="raspberry-router/#firewall-configuration"> the firewall section of my previous article</a> where I describe how to do so.

# Conclusion

I hope this has been helpful on any wondering geek that had trouble with this.
In the next article of this small series I will be getting more onto equipping your Raspi router with a VPN connection for additional (and almost mandatory) web browsing safety.

As usual: thanks for reading and feel free to share!



