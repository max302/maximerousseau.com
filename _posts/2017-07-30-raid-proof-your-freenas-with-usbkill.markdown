---
layout: post
title: Raid-Proof Your FreeNAS with USBKill
date: '2017-07-30 19:33:32'
tags:
- freenas
- usbkill
- counterforensics
- it
- is
- security
- zfs
---

By virtue of being a very multi-functional appliance in most settings, a FreeNAS machine has the potential to be a very sensible part of your rack. Even if you don't use jails or VMs to host anything, it's still a fileserver at the bare minimum, which means it hosts your data. I don't think I need to emphasize that protecting your data is important. FreeNAS being built on ZFS, the appliance integrates robust encryption from the get-go, but disk encryption alone leaves several attack vectors wide-open.

The most important of these attack vectors, as tragically highlighted by cases of Ross Ulbricht and Alexandre Cazes (of Silkroad and Alphabay infamy, respectively) is physical security. It's one of the first mantras of computer security I've memorized: "If you've got physical control over it, it's owned." Good luck getting any data out of an encrypted ZFS dataset on a powered down system, but on a live box? There are ways. By default, you can reset the root password from the console. Even if that's disabled, advanced enough intruders (read: state actors) have the means of poking and proding until they find a way. 

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9587657333914786"
     data-ad-slot="6494385734"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Short of [physically detonating your servers](https://www.youtube.com/watch?v=-bpX8YvNg6Y) upon detection of intrusion, there is still a way of minimizing the possibility of intrusion by using tools like USBKill. 

The [original USBKill was written by Github user hephaest0s](https://github.com/hephaest0s/usbkill) with the specific case of Ulbricht in mind, and is basically a USB peripheral watchdog that shuts down your machine as soon as it detects a change in connected peripherals. With config file tweaks, you can also run any amount of arbitrary commands and clear swap memory and RAM. 

## Adapting USBKill to Freenas
Since USBKill is comprise of a single file of Python, and officially supports FreeBSD, there were high chances that USBKill would work out of the box with FreeNAS. Unfortunately, FreeNAS does not ship with `lsusb` which is a used by USBKill, so it had to be adapted to use `usbconfig` which does come preinstalled.

My hacked up fork of USBKill for FreeNAS uses parsed output of `usbconfig` to monitor USB peripherals in the same manner as originally intended. 

 **Please note that I have not given any attention to the ram/swap clearing fonctionality. It may or may not work with what is already on board a stock FreeNAS install.**

You can find the [full repo on Github here](https://github.com/max302/usbkill/), but essentially, only the [usbkill.py](https://github.com/max302/usbkill/blob/master/usbkill/usbkill.py) file has been modified. 

##Setting up USBKill on Freenas

Writing to system folders like `/etc/` isn't supported by FreeNAS, any changes will be wiped out on reboot. In order to add stuff to the base OS, you'll need add stuff to `/conf/base/etc`. In my experience, everything within this is upgrade-resistant too, although it's probably a good idea to double-check after major upgrades. Add the necessary files with the following commands: 

``` mkdir /conf/base/etc/usbkill ```
``` cd /conf/base/etc/usbkill ```
``` wget https://raw.githubusercontent.com/max302/usbkill/master/usbkill/usbkill.py ```
``` cd /etc/base/etc/ ```
``` wget https://raw.githubusercontent.com/max302/usbkill/master/install/usbkill.ini ```

Now, we'll need to have the script start on boot. To do this, we'll pass multiple commands in a one-liner through the [built-in init/shutdown script options](https://doc.freenas.org/9.3/freenas_tasks.html#init-shutdown-scripts). Just add this line once the files have been put in place:

``` bash -c 'python /etc/usbkill/usbkill.py > /dev/null 2>&1' &  ```

Once you have save the init command, reboot. Then, you can test functionality by looking for the process we have asked to be launched with `ps -aux | bash`. You'll also get a log at ``` /var/log/usbkill/usbkill.log ``` that has timestamps. If the times line up with your last reboot, it's working.

You'll notice that we're calling on bash, which is an alternate shell on Freenas. This simply makes redirecting output to nul easier; I haven't had great success with csh.

Here's a demo on my own FreeNAS system. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/hej6ycgxWYo" frameborder="0" allowfullscreen></iframe>

## Pre-kill commands

Depending on your use case, you might want to run certain pre-kill commands to cause even more grief to whoever is trying to plunder your server. Here are a few ideas: 

* On an encrypted volume with no passphrase, or even on a volume with a passphrase, you may want to wipe the master AES key and your password encryption secret. These files can be found at `/data/geli/xxxxxxxx.key`and `/data/pwenc_secret` respectively. Deleting the files outright will work, but replacing the data with something plausible yet non-functional can cause even more confusion. 
* Without completely overwriting your disks, you can also quickly prevent an array from ever remounting by clearing the ZFS metadata. Check [this blog post](https://icesquare.com/wordpress/freebsdhow-to-remove-zfs-meta-data/) to learn how to do this.
* Notify yourself. If you mail alerts are correctly configured throught the FreeNAS GUI, using `mail` or `sendmail` to send yourself a notification should be a one line ordeal. 
* Shutdown everything. Since python is accessible, it's relatively easy to script stuff like IFTTT triggers or basically anything else you can think of. Set your home to lockdown mode, alert friends, mix and empty out your crypto wallets... with a little bit of code, you can do it all.    


##FreeNAS-specific additional considerations

In addition to the other [network security measures that I've gone through in this post](https://maximerousseau.com/home-network-segmentation-nat-loopback-to-vlan-on-ubiquiti-unifi-gear/), there are a couple of other things that you can do to protect your install of FreeNAS.

* **Disable serial consoles.** Does anybody actually use these things? They constitute an additional attack vector yet provide little to no actual functionality. Disable all serial ports in BIOS. If you still use serial... I don't even know what to tell you. 
* **Update and lock down your IPMI.** IPMI has been show to be a [serious vulnerability](https://www.wired.com/2013/07/ipmi/) in the past. Use a seperated VLAN, strong password, non-standard username. This is one of these things that WILL be defeated with physical access, unfortunately. However, most IPMI implementations connect virtual USB mouse and keyboard peripherals once remote consoles are opened; this means USBKill will catch any such attempt at remote administration unless the devices are whitelisted. See the [usbkill.ini](https://github.com/max302/usbkill/blob/master/install/usbkill.ini) file for details on how to whitelist peripherals. 

If I have forgotten anything, please let me know through email at max @ thisdomain.com





