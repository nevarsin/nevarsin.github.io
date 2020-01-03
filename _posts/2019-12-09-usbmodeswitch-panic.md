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

