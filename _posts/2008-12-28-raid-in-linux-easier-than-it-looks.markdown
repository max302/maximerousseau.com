---
layout: post
title: 'Raid in Linux: Easier than it Looks'
date: '2008-12-28 19:52:06'
tags:
- all
- ich9
- ich9r
- linux
- motherboard
- p35
- raid
- seagate
- software
---

Disk redundancy is something that I've been wanting in my home server for quite a while now, and since I now make (relatively) huge paychecks for working somewhere else than McDonalds, I thought that with the traditional HDD's prices plumetting because of the increasinly competitive SSD segment taking over, it might be a good time to invest in some RAID1 for protection. I took a deep breath, swiped my debit care, and 200 dollars later I was the pround owner of two Seagate 7200.11 500 gig drives. 

I thought that the ICH9r southbridge on my server motherboard would make things really easy: get into the BIOS, edit a few things, press next and ok a couple of times, and maybe install a driver or two with yum to get the damned thing working. But, it turns out that RAID controllers built into motherboards kinda sucks, and that compared to a real dedicated RAID controllers, they offer no advantage versus software RAID, also called softraid by some. After some further reading, I discovered that the only real perk to chipset raid is the ability to install an OS on a RAID array without being needy of a dedicated controller. Otherwise, it's pretty much the same thing as software RAID: since there is no dedicated RAID chip controlling where the data flows, the CPU still have to take care of all the operations concerning storage, so there are few to very little gains in terms of performance. Being to poor to afford a dedicated controller that cost in the <a href="http://www.newegg.ca/Product/Product.aspx?Item=N82E16816102085">200$ range</a>, I decided to go the softraid way instead. 

What I thought would be a long and tiresome process turned out to be super easy. Linux being super awesome, it has supported software RAID for quite a while, so setting up an array with a recent distro is so easy anybody with minimal skill at the command line can created their own.

First, you plug in your disks (duh), then install the actual software that creates and manages your arrays, called "mdadm". If you're under a Red Hat based distro or if you used yum for your software managing needs, just hit up the following at the command line.
<blockquote>yum install mkinitrd mdadm</blockquote>
After that, load a couple of modules:
<blockquote>modprobe raid1
modprobe linear
modprobe multipath</blockquote>
Note that in the previous example, I only loaded a few RAID types, because I was only planning on using RAID 1 anyways. Once that's done, format  and partition all of your new raid disks with fdisk, <a href="http://www.ehow.com/how_1000631_hard-drive-linux.html">as explained in this tutorial</a>. You'll want to use fd as partition type, which corresponds to Linux RAID autodetect. Once that is done, all you need is two commands to create your new array. 
<blockquote>mdadm --create /dev/md0 --level=1 --raid-disks=2 /dev/sdd /dev/sde
mdadm --examine --scan &gt; /etc/mdadm.conf</blockquote>
The first command will create you array, and from then on you will have a virtual disk called md0 in your /dev/ directory, which can then be mounted wherever you wish. The second command echo's your RAID configuration in a config file, so that your RAID array is initiated on boot. The devices list above are just the examples I used, use whatever values fit your situation. From then on, the disks will start a bit per bit sync, which according to the drive capacity and how powerful your machine is will take a while. You can monitor your array using the command "cat /proc/mdadm". Once the sync is done, you can go ahead and do all the regular stuff you will do with a drive, like create files system on it, mount it, add it to fstab for automatic mounting, and there you go, a fully redundant array. You may have to create a filesystem on your created array at some point, which if you followed my examples and common device mounting practices will be mounted at <strong>/mnt/md0</strong>. Just google up the mkfs command, and create an ext3 filesystem on your device. 

So far, I have only run into one problem, and that was self-induced. After forgetting to save the raid array's configuration in a file before rebooting, I attempted to recreate a similar array using <strong>/dev/sdd</strong> notation instead of <strong>/dev/sdd1</strong>. I thought I have rewritten all my drives while they were syncing, but turns out the only thing I messed up are the partition tables, and after rewriting the tables in fdisk and properly reconstructing my array, I had my data back. Big thanks to Google and <a href="http://blog.lostentry.org/2007/12/mdadm-devhdb1-is-too-small-0k.html">this blog entry</a> which helped me get my data back!

Now that I'm running disk redundancy, I sleep better at night thinking that my 4 year's worth of torrenting is safe. Or not.