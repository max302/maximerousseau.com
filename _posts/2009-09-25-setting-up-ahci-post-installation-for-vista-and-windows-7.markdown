---
layout: post
title: Setting up AHCI Post-Installation for Vista and Windows 7
date: '2009-09-25 00:24:58'
tags:
- '7'
- ahci
- all
- computer
- controller
- drive
- hard
- hard-drive
- hardware
- hdd
- how-to
- installation
- post-tag
- registry
- sata
- technology
- vista
- windows
---

I've got a confession to make. Throughout the 5 (or more?) systems that I have built throughout my relatively short period of interest in computers, there has been one thing that I have always omitted to enable, on every single system: AHCI. <a href="http://en.wikipedia.org/wiki/Advanced_Host_Controller_Interface">Advanced Host Controller Interface</a>, as it is otherwise known, is one of the higher level layers of the SATA norm that enables better control over hard-drives connected to a SATA controller. Concretely, using AHCI enables features which are unavailable with standard IDE controllers: Native Command Queueing (NCQ), finer grained power management options, and hot-plugging.

Power management is something that is pretty self-explanatory, and NCQ and hot-plugging are terms well known by most computing enthousiasts, but just for the heck of it, here is a short reminder on what those things do: hot-plugging allows for the instant recognition of SATA drives plugged in a running system; it kind of makes your storage supports act like USB peripherals: you plug it in, the OS recognizes it instantly. This was previously impossible with normal IDE controllers, and although hot-plugging works on SATA in IDE mode, it isn't totally automated: you still have to ask Windows (or whatever your OS) to look for it. As for Native Command Queueing, it is kind of like QoS filtering on a network, only it's for your hard drives. The feature reads ahead on it's tasks of commands, and prioritizes tasks as to minimized the distance traveled by the R/W head. It sounds kind of complicated when described with words... have a picture:

<img class="aligncenter" title="NCQ" src="http://expertester.files.wordpress.com/2008/07/300px-ncq_svg1.png" alt="" width="300" height="212" />It might not seem like much... but in a mass-multi-tasking scenario with very full disks, you can obviously see the need for this kind of technology.

Looks good eh? I've got bad news though: enabling AHCI in your BIOS will results in a <a href="http://en.wikipedia.org/wiki/Bsod">BSOD</a> on boot. Since AHCI is something entirely different from IDE, Windows does not normally have the AHCI drivers loaded, hence why your disks are unreadable. Normally, under Windows XP, one would have to reinstall the OS in order to get AHCI working, however, Windows Vista and 7 have "native" support for AHCI and therefor can be set to use and AHCI storage system even after the OS has been installed. I myself have had difficulty finding out how to install the proper drivers, until I reinstalled Vista on my main box recently, where I realized that I was probably missing out on some pretty good stuff. Better late then never, I learnt how to do it, and thought I might share.

What needs to be done is that the AHCI driver has to be enabled at boot, as for the OS to load properly. This can be done by enabling the generic Microsoft provided AHCI driver. All this requires is a little one key registry tweak, <a href="http://support.microsoft.com/kb/922976">as detail in this KB article</a>. Simply changes this key:

<strong>HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Msahci</strong>

So that it is equal to 0. Mine was equal to something like 4 originally. If I had to take a guess as to the nature of this number, I'd probably say that it has something to do with the driver load priority, where anything tagged 0 is loaded first, 3 last, and 4 disabled.

Anyways, once that's done, you may reboot your computer, go into the BIOS and set SATA MODE to AHCI. Your computer SHOULD boot fine, it may take a while to startup though. Once you are booted to your desktop, the "Found new hardware" dialog will go tits up, and pop up 6+ new pieces of hardware: each one of the SATA ports, the AHCI controller, as well as all the devices that you have hooked up on SATA. In my case, recognizing all this hardware took about 5 minutes.

Once that's done, you'll have to install a driver more specific to your motherboard, as the vanilla driver is far from optimal. Get the driver from your manufacturer's web site, install the sucker, then reboot.

Once that's done, you'll have to go through the process of waiting for the hardware detection wizard to do it's thing. If all goes right, you'll see all the stuff you saw last time it ran, plus your SATA controller listed under it's proper name. Once that's done, congrats! You're running off AHCI!

Now you can sleep easy knowing that your box (or at least your hard disk) is running full throttle. Enjoy!