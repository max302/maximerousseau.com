---
layout: post
title: Set Gmail as Your Default Mail Client
date: '2010-01-18 14:28:09'
tags:
- all
- browser
- chrome
- client
- default-browser
- email
- gmail
- google
- internet
- mail
---

To me, mail clients are a thing that belong to the old internet, alongside all the other nasty Web 1.0 garbage: animated gifs of rotating type, cheesy repeating wallpapers and ugly text formatting are amongst those. I've been using GMail ever since I was OH-SO-LUCKY to receive an invite from a tech-savvy buddy of mine who kept on raving about how awesome the service was. Ever since, it's been my only mail client; I've had maybe a dozen different mail accounts ever since then, but I accessed them all through Gmail. 

To me, it was clear that Gmail was my mail client of choice, however there didn't seem to be an easy way of telling my computer that in order for it to stop opening Windows Mail/Outlook whenever I clicked on a mailto link. So I did a bit of research, and cooked up my own solution to the problem, a little batch script that I call <strong>Gmail as Default Mail Client</strong>!

This little piece of batch does away with all your Gmail woes, and tells Windows to use the most awesome mail service whenever it encounters a mailto link. The script is set as to open Gmail as a new tab in <a href="http://www.google.com/chrome">Google Chrome</a>, everybody's favorite browser. This is done through a single registry edit that associates the mailto: tag to a new tab of Gmail. Unlike the Firefox quick fix which only works from web pages within Firefox (and forces you to use an inferior browser), this fix works system-wide with any program. 

As a bonus, I have also included a Gmail icon, which installs and associates automatically. That way, you can create cool .ink shortcuts to mailto links!

<p align="center"><a href="http://maximerousseau.files.wordpress.com/2010/01/mailto.jpg"><img src="http://maximerousseau.files.wordpress.com/2010/01/mailto.jpg" alt="" title="Mailto Example" width="223" height="79" class="aligncenter size-full wp-image-337" /></a></p>

Installation is super easy... run the .bat file, and you're done! You might have some security dialogs pop up since I'm playing around in C:\Windows to store the icon, you'll have to trust me on that one and click OK if you want everything to run smooth. 

<a href="http://www.mediafire.com/?jddxzd4ugd2">Download the script package here (Mediafire)</a>. 

This script is very easy to port to other browsers, feel free to modify and redistribute. I haven't found a way of starting the default browser instead of just Chrome, I'll be checking that out. I've tested my script with both Vista and Windows 7, and both systems worked. I am unsure about XP though. 

If you have any questions of problems, post in the comments with your email, we'll check it out. 
 