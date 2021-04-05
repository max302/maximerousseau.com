---
layout: post
title: Downgrading iPhone Firmware from 3.1.3 to 3.1.2
date: '2010-03-27 02:31:20'
tags:
- 3-1-2
- 3-1-3
- all
- apple
- downgrade
- downgrading
- firmware
- flash
- hardware
- iphone
- ipod-touch
- jailbreak
- tech
- technology
---

It was a pretty long chain of unfortunate events that led me to accidentally upgrade to 3.1.3. I was sitting in my couch one day, watching TV, eating random junk food. My iPhone was running a jailbroken 3.1.2 with all my favorite apps. Life was good. And then I get a text message... I recognize the signature at the end of the message, but the caller ID is a phone number instead of the name it's supposed to be. Puzzled, I fire up my contacts; All of my contacts are gone. I fire up the calendar; Nothing there. Same thing for the mail. So by now I'm wondering what is going on, try to reset my <a href="http://www.google.com/mobile/sync/">Google Sync</a> setup, to no avail. 

In a last effort to get my stuff back, I stepped in the trap that Apple set for me, and pressed the Restore button. I told myself, hey, there's a tool to JB that version anyways, right? Nope there isn't. I really want to stress this point: <strong>THERE IS NO GOOD JAILBREAKING TOOL FOR FIRMWARE 3.1.3!</strong> There is <a href="http://ih8sn0w.com/index.php/welcome.snow">one tool</a> out there that claims to hack previously jailbroken iPhones/iPod Touches by injecting a modified firmware image, but I and many others have not found any success. While the jailbreak seemed to have worked, I could not get any cellular reception from Rogers, which kind of defeats the purpose of having an iPhone in the first place. Don't be lured by the <a href="http://en.wikipedia.org/wiki/IPhone_OS_version_history#3.1.3">very minor bugfixes</a>, 3.1.3 is no good, no matter from what angle you look at it. 

So there I was, stuck with a bone-stock iPhone. I might as well have dumped it in a lake, because as everyone knows, once you go jailbreak, the ugly and inefficient stock iPhone UI is unbearable at best. Something had to be done... but I had no idea what to do. Downloading firmware 3.1.2 and force overwriting it to your phone will not work, because with all the latest versions of iTunes, Apple has to authorize your reflash. 

BUT! There is a but. I was able to reflash my phone to 3.1.2 using a pretty simple procedure. This will take a bit of your time, and it will probably be pretty scary, but it's well worth the ability to jailbreak again. Please not that I am not responsible for whatever happens to your phone. Try this at your own risks only, and good luck on getting an RMA on a fucked up phone if it were to happen. 

Follow the following:
<ol>
	<li><strong><a href="http://www.oldapps.com/itunes.php?old_itunes=46">Get iTunes 8.1.2</a> and install it.</strong> Try using a spare computer if you can, because the older version will not want to install with iTunes 9 on your computer. You can always uninstall 9 and reinstall the other version... but then you get all the associated problems. Whatever, just get a working copy of 8.1.2.</li>
	<li><strong>Fuck up your iPhone with Blackra1n.</strong> Yup, that's right, you'll want to voluntarily brick your phones software with <a href="http://blackra1n.com/">Blackra1n</a>. Chances are that if you're not totally new to jailbreaking, you know Blackra1n; it's probably the easiest to use jailbreaking app out there, but sadly it only works with 3.1.2. Run it nonetheless, and your iPhone will fall into a boot loop.</li>
	<li><strong>Plug in your iPhone to your computer and force restore to 3.1.2.</strong> Don't ask me why, but with your iPhone fucked up beyond repair, iTunes will skip the authorization process and install just about whatever valid firmware you feed it. It is still a good idea to turn off your internet while you are flashing. Shift click on Restore and point it to the firmware image for 3.1.2. Installation should be a tad longer, but otherwise pretty much to the point. </li> 
	<li><strong>Once your iPhone is restored, jailbreak once again.</strong> It's that easy. Blackra1n should now work like a charm. At least it did for me. The baseband update didn't seem to affect BR's potency, although you can now forget any potential unlocking.</li>
</ol>

The latter worked for me in a Windows Vista SP2 environment with an early model iPhone 3Gs, and while I can't confirm that it will work on all platforms with all devices, I don't see why it shouldn't. After all, the iTunes function which allows any unverified restore if the phone is a goner could very possibly be a planned customer care feature. But then again, since when does Apple do stuff for it's customers? 

That being said, if you're not necessarily trying to get a quick fix, you might want to see if the steps work with the latest version of iTunes. 

Pinging me back in case of success would most likely be very appreciated by both me and the jailbreak community as a whole. Drop a comment, copy to forums (with a link of course, net etiquette requires), pingback from your blog, whatever. As long as this info can get more people jailbroken, then I'm a happy man. 
