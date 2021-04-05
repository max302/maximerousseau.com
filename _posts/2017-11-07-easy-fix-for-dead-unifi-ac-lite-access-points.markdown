---
layout: post
title: Easy Fix for Dead Unifi AC-Lite Access Points
date: '2017-11-07 01:21:09'
tags:
- ubnt
- ubiquiti
- networking
- no-power
- ac-lite
- repair
---

I've been deploying Ubiquiti's Unifi gear for a while and I absolutely love it. While the development cycle of firmware and software is somewhat uneven, the products are great, inexpensive, run off a self-hosted management portal and don't have any licensing costs attached to them. 

Except for firmware-induced hiccups, I haven't heard anything more than anecdotal evidence of systematic failure or any product in their line. One dealer told me of an issue with early SKU 24 port 150W switches' POE modules, but that's about it. I personally have had my first failure on ANY Unifi product this week when a UAP-AC-LITE shit the bed, failing to power-on with any POE source. Other symptoms included flashing light on the passive power injector, and a short flash of the white LED if the reset button was held in with the power-injector connected.  

Turns out the problem has already been documented, and comes from a TVS diode in the power circuit that blows up; [a kind fellow on iFixit has already documented a fix](https://www.ifixit.com/Guide/Ubiquiti+UniFi+AP+AC+Lite+TVS+Diode+Replacement/73360). Sure enough, this is exactly how mine broke. 

The guide says that the diode is there to protect other components, but it's unclear if the failure is due to a cheap component or if it is provoked by some particular incident. In my setup, the AP was powered by a Unifi US-8-150W switch, which itself was connected to a high quality UPS; the likelyhood of a power-related incident is low. There has been an acknowledged bug in firmware in the past that has made certain 24V devices get 48V during reboots, but this problem is long since fixed, or at least this was my impression. [This post](https://community.ubnt.com/t5/UniFi-Routing-Switching/UniFi-Switch-24v-Passive-PoE/m-p/2103593/highlight/true#M61755) seems to indicate that there is no fix. Having missed the [original announcement and the giveaway of 802.3af adapters](https://community.ubnt.com/t5/UniFi-Wireless/Warning-UAP-AC-LITE-and-UAP-AC-LR-with-UniFi-Switch-on-PoE/m-p/1771809#U1771809), I've missed the boat. 

I've reached out to UBNT for warranty support, and they don't seem to have any intention to warranty this kind of fault, or to admit that the diode blowing up is the expected mode of failure. 

This might be good news for deal hunters with connections to recyclers. Considering that the AP is the lowest-end AC offering in the Unifi lineup and that there are quite possibly tens of thousands of affected units in the wild, a spool of diodes for a few bucks and a cheap broken APs might be a terrific score for somebody intending to use the APs without a POE switch, or with a seperated 802.3af adaptor. The AC-Lite was a terrific deal at it's inital MSRP, and remains one of the most competitive offerings even in it's later 48V, 802.3af compliant version priced the same at around 100$.  