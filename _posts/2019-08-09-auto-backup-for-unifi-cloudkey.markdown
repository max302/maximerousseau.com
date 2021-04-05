---
layout: post
title: Auto Backup for Unifi CloudKey
date: '2019-08-09 17:15:09'
tags:
- unifi
- ubiquiti
- rsync
- cloudkey
- backup
---

Working with Ubiquiti gear is a wild ride; the ecstasy of deploying enterprise-grade gear for the borderline absurd prices that the Edge and Unifi lines enable is all to often ruined by the agony of buggy firmware, features that were promised on GA releases that are still not implemented 2 years later, and questionable business decisions regarding certain product lines. One of the recent frustrations I've had was[being forced into buying a CloudKey+](https://help.ubnt.com/hc/en-us/articles/360011480554-UniFi-Protect-FAQ#5) in order to run a fully updated version of Unifi Protect, after years of fully self-hosting all my stuff.

While the bugs that I encountered since the 1st gen. CloudKey days seem to have been fixed, the problems caused by having a physical appliance running the controllers still presents problems regarding backups and service availability in case of a hardware-related issue. While all controllers now have an autobackup feature, the backup files are still hosted on the appliance; this really isn't a backup in any meaningful sense of the word.

If you want any sort of safety at all, you need to get those files off the appliance. ‌‌‌‌Luckily, the CloudKey is basically just a fancy ARM64 computer with a custom image of Debian on it, and it is still fairly extensible, so there are plenty of ways to pull the autobackup files off of the device.   
  
Since the autobackup file retention is managed from within the Unifi controller, all we really need to do is keep an updated copy of those files somewhere else, so I instinctively tried to run an RSYNC over SSH on the device. RSYNC failed, as the service isn't installed by default on the Cloudkey, but installing this service is as easy as on any other linux device, as the regular Debian Stretch repos were not removed from the appliance. Get root SSH access and run the following commands:

> apt-get update && apt-get install rsync

From there, you'll need to configure the SSH access via password-less authentication on the CloudKey to enable rsync to do it's job.

> cd /root  
> mkdir .ssh && chmod 755 ./.ssh  
> touch ./.ssh/authorized\_keys && chmod 644 ./.ssh/authorized\_keys  
> service sshd restart

On the machine that will be fetching the backup files (in my case, a FreeNAS box), copy your public key to allow password-less access: &nbsp;

> ssh-copy-id root@INSERT\_CLOUDKEY\_IP

You can now pull backups in from another machine to grab the backups. [FreeNAS has a GUI utility to](https://www.ixsystems.com/documentation/freenas/11.2/tasks.html?highlight=rsync#rsync-tasks) help &nbsp;you build RSYNC jobs, but a cron job would work just as well. The paths for the auto-backup folders are the following:

> **Unifi:** /srv/unifi/data/backup/autobackup/  
> **Unifi Access:** /srv/unifi-access/workspace/backup/  
> **Unifi Protect (configuration)**: /etc/unifi-protect/backups/  
> **Unifi Protect (video files):** /srv/unifi-protect/video/

These paths only backup the configurations. Seeing that a lot of the problems I've had in the past with the Unifi controller was related to the MongoDB backend, I decided that it was probably wiser not to try pulling files from a live database. If the Unifi service was stopped, I don't see why you couldn't copy the entire application folder, logs, database and all.

