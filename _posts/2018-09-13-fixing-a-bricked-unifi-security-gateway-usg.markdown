---
layout: post
title: Fixing a Bricked Unifi Security Gateway (USG)
date: '2018-09-13 01:37:17'
tags:
- unifi
- security
- gateway
- networking
- repair
---

I thought I was being real smart when I hooked up an eBay-bought 802.11af-compliant 12v POE extractor to my Unifi Security Gateway. The whole idea was to get rid of all the power-warts and AC-DC conversion devices hanging off my UPS to make the makeshift rack in my parent's basement cleaner, in addition to making my router power-resettable through the Unifi control panel. 

This genius scheme worked fine for the better part of a year, before this summer's grueling heat took it's toll on the extractor. Naturally, this all took place while I was thousands of kilometers away; luckily, power resets and cooling off the adaptor worked for a while, but at a certain point it gave up the ghost entirely, still drawing current from the switch but making a horrible pulsing sound. 

The erratic power delivery was not very well received by my USG, which decided that it would no longer boot correctly, or really boot at all for that matter. The behaviour of the device was as follows:
* Interface lights would light up as per normal during the boot sequence
* Status LED would NOT light up at all, not even giving the usual boot-time flashing white
* As one would expect, the device would not ping or connect over SSH. A Wireshark scan also showed that it didn't seem to set the LAN interface IP either to it's last known provisionned value, or default value. 
* Any form of reset, including a 30-30-30 reset would do absolutely nothing.

Browsing the UBNT Community forums showed that this was most likely a corrupt USB drive. The firmware on these devices is stored on a 4GB USB stick plugged in internally, as pictured here. 

![IMG_2439](/content/images/2018/09/IMG_2439.JPG)

Luckily, I had a fresh, out-of-box sample USG with the board entirely out that I had bought to mock up in CAD for some bracket work I wanted to get done. I copied the fresh flash drive to my Mac using DD, imaged it back to the defective USB drive, plugged everything back in and got things working again. After about of minute of what is probably first-use initialisation on post-recovery power-on, the white light status light started flashing, and within normal booting time switch to solid, at which time the device had set it's default IP address on the LAN interface and was accessible through it's web UI. 

The USB drive has the following partition layout: 
![Unifi-USB-Drive](/content/images/2018/09/Unifi-USB-Drive.png)

I'm honestly surprised that it was decided to use a USB drive to hold the software. I've had tremendously bad luck with putting bootable software on USB drives; all my BSD-based appliances ate drives at a rate where I quickly found myself using 3 or 4 drive mirror setups to make sure that drive failure wouldn't break my stuff. However, the advantage is that I didn't have to break out a serial cable for some complicated console-based restore process, which is fine by me since just FINDING my serial adaptor would have been much longer than DD'ing a 4GB flash drive was. 

Recognizing the uncanny way in which I somehow dodged Murphy's Law by having a fresh spare on hand, I decided to thank the IT gods by making the image available here. Version info on the restore device told me this is firmware v4.3.33, the older-style firmware with less configurtion options available to an un-adopted device.

You can download the image [here](http://https://drive.google.com/open?id=1B4NQOPfr73h20nbkWwLqWpxrGINYdMLG).

Checksum is as follows: 

> MD5 (UniFiSecurityGateway.ER-e120.v4.3.33.4936086.161203.2031.img) = 83941494d4f9f9fb906e19b8ecf68696

You can DD this image directly without specifying any block size, which worked just fine for me.

**IMPORTANT NOTE ON COMPATIBILITY**: I am unaware of any potential incompatiblities with hardware or low-level firmware between the software I uploaded and whatever your USG might be running. What I know is that there did not appear to be incompatibility issues going from firmware 4.4.22 back down to 4.3.33. Use at your own risk. Both units used a PCB that was marked "Copyright 2014", despite being purchased years from one another. 

Bottom line: Power matters. If you want to centralize power, get a decent rackmount power supply in the appropriate voltages, and if control is what you want, get a relay setup. 