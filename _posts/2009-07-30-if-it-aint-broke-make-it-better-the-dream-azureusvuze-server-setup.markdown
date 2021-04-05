---
layout: post
title: 'If it Ain''t Broke, Make it Better: The Dream Azureus/Vuze Server Setup'
date: '2009-07-30 03:44:14'
tags:
- all
- army
- azureus
- career
- grub
- hardware
- linux
- rmc
- server
- setup-tag
- vuze
---

As you might know if you've been following my Twitter feed, your's truly is departing Sunday of this week for a 13 year long military career. This is obviously a very big step in my life, in two ways: I'm going in the Army, which itself will surely affect my lifestyle, career and outlook on life forever. But, leaving for the military also invovles moving out of the family home, and into the <a href="http://www.cmrsj-rmcsj.forces.gc.ca/index-eng.asp">RMC St. Jean</a>. Knowing that free time is a very rare thing at St. Jean, I decided to clean up a couple of things I had laying around my room, and finalized some long overdue projects, such as my <a href="http://www.overclock.net/case-mod-work-logs/506842-project-umbra.html">current case mod</a> which is getting done at a snail's pace, and a couple of routine maintenance operations on stuff on my home network.

One of the first things on my list was changing my home server's primary OS drive, which was previously a nearly 10 year old Western Digital Caviar 40 gig drive hooked up on IDE. It made sense to recycle the old drive way back when I was just starting to mess around with the idea of a home server, but now that the box hosts<a href="http://twitpic.com/1qjkx"> 5 other network share drives</a>, I really don't want to risk some downtime or data loss because of an old drive. So I bought a new SATA WD 160 gig drive, and initially set out to copy the old drive to the new one, using Acronis TrueImage.

During the copy, all went OK, but back in the system, the bootloader acted up, and with reason, because I was changing the OS from a drive recognized as /dev/hda to something more amongst the lines of /dev/sda. After numerous attempts at fixing the MBR/GRUB, I gave up, and downloaded the latest Fedora for a fresh install.

Turns out, I really should have thought of doing that beforing even considering a full migration to a new disk. Armed with the determination to get my shit working well once and for all before I head out to St. Jean, I started installing the new OS on the fresh drive. All went well during the installation, which was pretty fast (good job working on Anaconda, Fedora people), and apart from the fact that Palimpsest detected that three of my HDs were crippled with bad sectors, including both drives in my RAID-1 array dedicated to important stuff. Woo.

All hard drive failure aside, it really payed off to restore a fresh OS. For one, Fedora 11 seems WAY snappier. OK, it could be due to the fact that I'm on a much faster boot disk. REgardless, with a newer version OS also comes lots of updated software goodies, including a fresh new kernel (I hadn't updated mine since F9, I'm a lazy ass). When the fresh install up and running, I set out to get my network shares going. Nothing complicated there, as my RAID1 array was automatically detected, so all there was to do was to create my mountpoints (I used <strong>/storage/whatever</strong> for all drives meant to be shared on network), edit fstab, get the SMB config running, and I was ready to go.

Next up, <span style="text-decoration:line-through;">Azureus</span> Vuze was the tricky part. I download lots of torrents... enough to have my ISP calling me on a regular basis to remind me that they have a cap thing going on (which I'll discuss in a later article). So obviously, in order to download my stuff easily and efficiently, I have a pretty complex setup when it comes to Vuze.

My old setup, while more complex than necessary, was pretty ghetto. I basically consisted of an installation of Vuze that was called on by rc.local on startup, that ran in command line and shot all of it's output to null. To control it, I had to used the Swing Web UI, a plugin which gives the user a complete yet simple Java powered web UI. The ports for this UI were onlocked on my router, giving me the ability to upload and manage my torrents from any computer rocking Java and an internet connection. This feature in itself was pretty nice. Downloading, completed, and .torrent files were all stored on my torrent drive, a SATA 320 gigger which I had scored for free at work, given to me by a customer who's external drive had failed and who didn't want the internal drive back. Having a dedicated torrent drive helped reduce the load on the already pretty slow OS drive which I was previously using... at some points I had so many simultaneous read/writes going on that I started getting IO errors in Vuze which automatically stopped some of my downloads... not a good thing. Anyways, the torrent drive was then shared via Samba, and mounted on my computer(s), so I could manage, relocation and view/open files. I also had this thing going on where a folder on my network share was monitored by Vuze for any new torrent files to be added... nice for batch uploading.

The setup worked fine, but it really wasn't ideal. The startup script was a huge fail, because it would run Vuze in root, which apart from the obvious security concerns was pretty lame because any files created/manipulated would be marked as owned by root. This meant that anything on my torrent drive, regardless of if all the folders had been chmoded to 777 beforehand, was listed as read only to the user that Samba used. Removing old files was therefor a pain in the ass, as I had to SSH into the server to change the permissions to whatever files I had to manipulated. In the end, I had a script called 777torrents which I ran whenever I needed to manipulated, that did a recursive chmod of all the torrent files. While all of these quicky fixes worked without a hitch, it isn't something I wanted to do again on the new install.

So I set out to install Vuze. On Fedora, this was pretty easy.
<blockquote>yum install azureus</blockquote>
Yup, the Fedora repo still lists Vuze as Azureus... I'm not fond  of the new name anyways. Once that's done, there is some slight modification to some core Azureus files that need to get done in order to be able to run the application in text mode. Snatch the latest .jar file as well as complimentary modules <a href="http://azureus.sourceforge.net/index_CVS.php">from this page</a>, dump them in your Azureus install directory, and you should be good to go. In my case, the directory in question was <strong>/usr/share/azureus/</strong>.

At this point, Azureus will be ready to run. Try it out, customize the options to your liking, check out the plugins for added functionalities. For what I needed, I downloaded the Swing Web UI, the HTML Web UI, mod for the latter plugin called <a href="http://www.joeltron.com/izureus/">iZureus</a> as well as the <a href="http://www.openp4p.net/">P4P</a> plugin (also something I'll be discussing by next post). <strong>BE SURE YOU INSTALL THE PLUGINS WITH THE USER YOU WILL BE RUNNING AZUREUS WITH! </strong>Since configuration information and plugins are stored in the a .azureus folder in your home directory, not doing so will mean that none of your config will carry on to other users. I learned this the hard way the first time I ever messed around with Azureus. Keep in mind that to accomplish the "complete solution" kind of setup that I am sporting, the user using both Azureus and Samba will have to be the same.

Once Azureus is up and running like you want it to, close it, and <a href="http://www.azureuswiki.com/index.php/HeadlessSwingUIAtBoot">go read this wiki page</a> to get the instructions on how to start your fresh new torrent client on boot. Honestly, the instructions there are pretty concise, I don't know what else to add. Just remember to edit the script to reflect your environment, user, installation directory and all.

When that's done, the only thing left is to configure the shares for all of your files. Usually Samba comes with any installation of Fedora, but if something messes up or you're using another distro, you know what to do:
<blockquote>yum install samba</blockquote>
&lt;3 yum. As for the configuration of your new service, <a href="http://www.reallylinux.com/docs/sambaserver.shtml">ReallyLinux has a nice tutorial</a> (old a bit, but hey) on setting up the entire thing. Don't forget to use the same user that's running the Azureus screen daemon in the "Create Server Users &amp; Directories" part of the process, unless you REALLY want to make setting permissions on your torrent files a pain in the ass. You might want to consider creating a hierarchised torrent folder like I did, with the following structure:
<blockquote>-torrents
---complete (completed files get moved here)
---downloading (incomplete files go here)
---.torrents (all .torrents files are stored here, even after the torrents are removed)
---.toadd  (this is the infamous monitored directory in which azureus looks for new .torrent files)</blockquote>
Last step: change the ownership of all the shares you just created to reflect whatever user you took to run Azureus and Samba. This is usually a one command thing, it will look like this:
<blockquote>chown -R max302 /storage/torrents</blockquote>
Voila. The /storage/torrents/ folder and all of it's content is now mine. I kept the default 775 permissions, but you can always enforce something more radical once the folder is your property, 770 or 700 would make sense.

There you have it, a super clean, automated bittorrent server. There is only ONE thing missing from this setup, and that is a method for automatically moving .torrents from my browser's download folder directly to the .toadd folder... I've thought of a cron/at job that runs a batch script which checks the contents of the folder at regular intervals, but I just don't like the idea of things running periodically. I'm dreaming up a little program that could do it, but I'd have to learn to code first &gt;:\ .

Have fun, and remember, <a href="http://phoenixlabs.org/pg2/">pratice safe torrenting</a>!