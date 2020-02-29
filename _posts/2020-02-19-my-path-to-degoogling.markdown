---
layout: post
title:  "Privacy or security, choose one. My path to degoogling"
description: If you're concerned about privacy (and you should be) here's a quick way to route all your home traffic through a VPN"
date:   2020-02-20 13:30:00 +01
categories: life
author: nevarsin
published: false
---

#Struttura art 1
1) Introduzione, con fonti, al concetto di surveillance capitalism. Link a libro/podcast mozilla. Citazione Google e Facebook con link a casi eclatanti.
2) introduzione degoogling: fare a meno dei Gservizi - opensource
#struttura art 2
1) Status quo mobile 
2) Scelta di Android ma senza Gapps: concetto di alternativa stretta tra sicurezza (es. app bancarie) e privacy.
3) Guida al flashing + microG
4) Descrizione mia esperienza
#struttura altri articoli
1) Email (Runbox)
2) Fileshare + Contatti + Calendar (Nextcloud)
3) Maps

#Privacy or security: choose one.  De-googling.
This is the first article on my angle on current status of technology. A sad one, unfortunately. I will describe here how I decided to go Google-less on my mobile device and the subsequent implications.

## Mobile IT status quo
I don't need to elaborate on how the market is split between two major mobile OS: Android from Google and iOS from Apple.
Market share is pretty much 80/20. You know the angles:

* Android is available on a wide variety of devices (lowend to flagship) and is officially an OpenSource project (AOSP). It generally allows for more user freedom (e.g. third party app stores) and won't force you to a specific UI/UX context.
* iOS is Apple hardware only, closed source, walled gardened, etc.

Both have their overall pros and cons but this is not the topic here. The center of this post is how you are handling your data everyday to both (and more) vendors and what happens if you decide that you don't want that anymore.

## Business Model and Data collection

### Apple
Their business model is (mostly) clear: you are seduced to buy a new model every year and, in exchange for that, they provide you with best-in-class user experience and services. This includes some free services (Maps) and some paid one (Apple Music). 

They do not provide advertising services and they have a documented history of preserving data privacy even when pushed by the US government.

They may be blamed for opposing right-to-repair registration, forcing people to junk repairable devices and beeing not so green overall.

### Google
Google gets a "license fee" from each device that gets Goole Services and Apps installed. After that the almost entirety (?) of their services is provided to users for free (Google Maps, Translator, GMail, Youtube, Calendar, Docs, Sheet, etc). Have you ever asked yourself where the catch is?
 
Google business model is "shadier". They get almost all of their revenues from AdSense, their advertising service. 
You are thinking "Oh yes, those promoted search results that I never ever click". Yes, those.
It turns out a lot of people click them and generate revenue, especially given Google's progressive blending of promoted results among the normal ones*C.

Objective number one for Google is to improve the click ratio on those banners (even a 0,1% increase brings millions $ more) and they do that by making those ads as relevant as possible for you. Not for your search keyword. Not you as a category. Not you as a citizen of your nation. You.

## Collection
AI has progressed a lot in the last decade and, by big techs like Google and Facebook is pretty much used for one specific goal: sort out all the data coming from all the "free" services they provide and use it to profile your habits*C, your likings, your political orientation*C. 

This is done by collecting as much as possible of the so called behavioural surplus: everything you do **while** you are using any service: how often do you open the app, how much time you spend scrolling that specific result set, how long have you stopped scrolling to look at a specific content and so on. Evertyhing adds up and, together with technical info like your OS version, pc model or ip address, makes you unique. This data analysis discipline is called fingerprinting*C.

## Android
 
Android was designed as a permanently connected tracking and data collecting operating system. This means that, whenever you "Agree" on some terms without reading them, you are consenting to give away **a lot**. An incomplete list:
- browsing history (Chrome)
- physical movements (Maps)
- search habits (Google Search)
- phone usage habits 
- online purchases (Google Search)
- foreign languages usage (Translator)
- watched content (Youtube, Chromecast)
- photos (Google photos)

The list can go on

