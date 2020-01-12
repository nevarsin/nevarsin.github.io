---
layout: post
title:  "Raspberry Router part 2: How to VPN your whole network"
description: If you're concerned about privacy (and you should be) here's a quick way to route all your home traffic through a VPN"
date:   2020-01-09 13:30:00 +01
categories: linux
author: nevarsin
published: false
---
|![]({{site.baseurl}}/images/usbmodeswitch_header.png)|
|:--:| 
| *the default usbmodeswitch conf file* |

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
