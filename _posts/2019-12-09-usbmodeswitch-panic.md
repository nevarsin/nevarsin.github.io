---
layout: post
title:  "Raspberry, USB Modeswitch and Kernel panic"
description: USB_modeswitch, on Raspbian, is a bless. Sometimes, however, things can "panic". This is how I fixed it.
date:   2019-12-09 13:30:00 +01
categories: linux
author: nevarsin
published: false  
---
|![]({{site.baseurl}}/images/usbmodeswitch_header.png)|
|:--:| 
| *the default usbmodeswitch conf file* |

<<<<<<< HEAD
USB Modeswitch is a

# Alcatel X200/X200L/X060S/L100V, Archos G9 3G Key
TargetVendor=0x1bbb
TargetProductList="0000,0017,00b7,011e,0191,0195"
StandardEject=1



root@raspberrypi:~# cat /etc/usb_modeswitch.conf 
# Configuration for the usb_modeswitch package, a mode switching tool for
# USB devices providing multiple states or modes
#
# Evaluated by the wrapper script /usr/sbin/usb_modeswitch_dispatcher
#
# To enable an option, set it to "1", "yes" or "true" (case doesn't matter)
# Everything else counts as "disable"


# Disable automatic mode switching globally (e.g. to access the original
# install storage)

DisableSwitching=1

# Disable check for MBIM module presence and configuration globally (to aid
# special embedded environments)

DisableMBIMGlobal=0

# Enable logging (results in a extensive report file in /var/log, named
# "usb_modeswitch_<interface-name>" and probably others

EnableLogging=0


# Optional increase of "delay_use" for the usb-storage driver; there are hints
# that a recent kernel default change to 1 sec. may lead to problems, particu-
# larly with USB 3.0 ports. Set this to at least 3 (seconds) in that case.
# Does nothing if the current system value is same or higher

#SetStorageDelay=4



MessageContent="55534243123456788000000080000606f50402527000000000000000000000"
=======
# Introduction

This article is more of a spin-off of my tutorial on how to turn an old Raspberry Pi on a smart and free home router. I will describe 

# What is USB_Modeswitch and why do we need it?

USB Modeswitch is a simple but effective tool that, since 2010, helps Linux user/admins with those weird hybrid usb dongles.
Let me explain what I mean by that:
USB dongles come in a variety of forms and they are typically single purpose (a modem, device adapter, network interface, etc). What makes them weird is that, because of Windows and its extremely lacking set of device drivers, vendors had to find a way to supply said drivers in a plug&play fashion.

This means that, when you plug one of these devices for the first time, it will
- tell the OS "I'm a CDROM! Here's the setup you wanna autorun!"
- install the actual driver
- switch to its actual purpose 
- start working

Linux, on the other hand, comes with a whole load of device drivers included in the kernel, so no need for that extra step.

So every time you plugged in a bt/eth/wifi/whatever dongle in your workstation and saw it working right away, you have to thank usb_modeswitch (and the kernel devs, of course)
 
# 

>>>>>>> 52d8fcb36d49709a0967c40779b91fe1a9f22da2
