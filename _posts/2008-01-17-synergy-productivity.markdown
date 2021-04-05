---
layout: post
title: Synergy, Productivity++
date: '2008-01-17 00:03:40'
tags:
- all
- free
- multi-tasking
- open
- productivity
- sf
- software
- source
- sourceforge
- synergy
---

On a computer, we power users don't just do something, we multi-task, ALL. THE. TIME.  It's a known and documented fact, we like to juggle with multiple apps that do multiple things, on multiple computers with multiple displays. We want to, no have to, keep constant situational awareness on a shitload of RSS feeds, our 3 email addresses, 6 IM accounts, a couple dozen IRC channels and the newest funniest crap on Youtube, all this while writting LISP and the half complete English assignment that's due for tomorrow. It's a sort of megalomaniac, control freak aspect of <a href="http://www.randsinrepose.com/archives/2003/07/10/nadd.html">NADD</a> I guess.

However, problems arise when comes the time of controlling all of these machines simultaneously: there can only be so many keyboards and mice on a desk, and I've not yet seen 3 level keyboard trays. With a workspace containing 2-8 computers, <a href="http://www.tigerdirect.ca/applications/category/category_slc.asp?Nav=|c:202|">a KVM switch</a> is a viable options, but the price of those things is outrageous. Another possibility remains: using the connection to your home network as a bridge between your desktops with <a href="http://synergy2.sourceforge.net/">Synergy</a>, the software KVM.

Synergy is an open source tool hosted at Sourceforge which does the exact same thing as those multi hundred dollar hardware KVMs, and even a bit more, using your existing network as a medium. For the average basement geeksta who scrapes the pennies to afford his new toys, this is really a blessing. How it works is pretty simple, you basically need 2 components to get Syngery working. The server, which is the computer which shares it's keyboard and mouse, and that holds a record of what machines are physically next to each other, and the client, which connects to the server and is totally independent until the mouse from the server moves off it's own screen into the said client machine. You get the transition that you usually have when running dual head monitors, but each machine is totally independent apart from that. This means that you can go from making some 3d design on one computer,  to playing your music on the other, to sorting your photos and replying to emails on yet another, in one quick sweep of your mouse. I've used it countless times to switch quickly between my laptop for MSN and reading RSS to my main rig for power-browsing and work on Google Sketchup.

If you own many machines on many OS's, fear not: Synergy has a release for Windows, Linux and MacOSX, so managing your Linux server no long means finding a spare keyboard and crawling under your desk. Since it works in tandem with X, runlevel 3 and under won't work though.

It has 2 drawbacks though. The first one, major concern for those of you planning to use this in a LAN with potentially devious users, is the lack of encryption. This means that using Synergy in a hubbed network environment means turning all your machines into multicasting keyloggers. Tunneling is a possibility, but it isn't really accessible to all users. The second is that there is some slow-downs related to sending timing critical information such as mouse movements over a network, which makes it almost impossible to use in standard wireless G or hubbed environments (security problem solved!). With a wired server and a wireless client, I was able to use some apps like browsers and IM clients, but such things as image edited on a wireless client isn't something I would recommend; as soon as the wireless client puts a slight bit of stress on it's connection, you can start feeling it in the movements of your mouse. On well built 100 megabit networks with good quality switches and low latencies though, you shouldn't have a problem.

<a href="http://synergy2.sourceforge.net/">You can download the latest version of Synergy, 1.3., from it's page at Sourceforge</a>. It's pretty old, not having been updated for almost a year now, but it still gets the job done, and I can certify that it is Vista x64 compatible, as long as it's run as administrator.