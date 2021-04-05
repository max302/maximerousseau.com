---
layout: post
title: One More Thing Linux is a Bitch For...
date: '2007-08-27 16:16:37'
tags:
- all
---

Printing.

Back in the days where good ol LAGGBOXX used to run it`s comfy Windows, I used to create network shares like there was no tomorrow. Share this folder with read access to dump my files, share that with view only access to see what my family was storing. Of course, there is a significant lack of security when creating this kind of share, because may they be modifiable or not, there is absolutely no way of regulating who gets to play around with those shares. Linux is great for that: easy to configure, password protected (or not), very flexible SMB sharing of anything you want, as long as its a directory.

Then comes in printing and ruins the whole thing. I know that some hardcore linux users will probably lol seeing me say that, but I was expecting a click-click-yes-ok-enter kind of process for sharing a printer, pretty much how sharing files work. Yes, there is a nice little "Sharing" tab under the Fedora Core Printer Managing tool, but really, I doesn't do shit. Turns out that linux printing is much more difficult that.

Because of the multilevel aspect of printing and sharing, all components included in the printing and sharing process have to be configure, which includes but is not limited to: Samba, CUPs, and HPLIP in my case.

The first step is really, really easy, and it consists in installing the printing device. I don't really know about other Linux distributions, but as for what concerns me, I found that the supported printer list was HUGE. My HP PSC 1510xi all in one was quickly detected install, but without the scanning functions, that's a whole other thing. Once it's detected, I went on to share it with the printer manager tool, which didn't appear to do shit. I then turned to more drastic configuration methods, the dreaded configuration text edition. Using <a href="http://www.redhat.com/docs/manuals/linux/RHL-8.0-Manual/admin-primer/s1-printers-sharing.html">this page as resource</a>, amongst others, I used this as SMB configuration:
<blockquote><code> [printers]
comment = All Printers
path= /var/spool/samba
security = server
printer = raw
browseable = yes
guest ok = yes
writeable = yes
printable = yes
use client driver = yes
guest account = nobody
create mode = 0700</code></blockquote>
According to what I have read, this was supposed to show my printers on my network's SMB shares... and it did. But then when trying to connect to the printer, I was given a "permission denied" message. Being back from a party and still being in recovery from an intoxication, and very grumpy, this stupid message was driving me insane, specially since I found my SMB printer config pretty nondiscriminatory when it came to letting people access the printers: guest ok was on, as were all the other options which were most likely to cause problems. At the time, little did I know that CUPS, the printing system for Linux had it's word too on who gets to print. Only the <a href="http://www.redhat.com/docs/manuals/linux/RHL-8.0-Manual/admin-primer/s1-printers-sharing.html">Red Hat article</a> on printer sharing even mentioned those config changes, which are vital for proper printing in raw format. Now that I got everything properly configured everything configed nicely, it'll work I tell myself, tired and frustrated. I whip out trusty notepad, writing a couple of nonsense lines, send it off to print. Behold, it works. By that time, I am celebrating my great success. But then my father had to come in and print an HTML page. The started off normally... but halfway through the job, the printer just crashed, stopping all activity and flashing only the power LED.  I fucked it off and went to sleep.

I'm now in the process of reinstalling HPLIP, updating CUPS and probably SMB too if my attempts fail again, but this is one thing that Linux developers might want to work on in order to get linux in the mainstream desktop OS scene. It's a saddening thing, but most users, including me, have been used to simple resource sharing and getting things to work with only one application, one configuration. In the meanwhile, I guess I'll have to crack my skull on more browsing and text configuration to work, but for this is really not what I would call average user friendly.