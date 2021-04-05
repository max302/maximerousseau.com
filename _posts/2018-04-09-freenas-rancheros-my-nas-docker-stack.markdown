---
layout: post
title: FreeNAS + RancherOS, my NAS Docker Stack
date: '2018-04-09 01:14:41'
---

I've been a FreeNAS user since the 8.x days, having decided to ditch a self-hosted file server based off Fedora with a pretty basic MDADM + ext3fs storage setup. Running on commodity hardware, the prior setup was fine, but little other than an extensible NAS that was somewhat of a hassle to manage and keep updated. FreeNAS and it's plugins jails promised to do most of what I was doing beforehand, in an appliance format that is much easier to maintain.

FreeNAS Corral, aka FreeNAS 10, was supposed to bring native Docker support to the product with the help of a built-in Bhyve + boot2docker setup built right into the new UI. While jails are technically as flexible as FreeBSD and the [ports library](https://www.freebsd.org/ports/) are, plugins-jails are a bit more complicated and you are generally dependant on a maintainer for updates. Case in point: as of my writing of this, the PBI jail for Owncloud is stuck at 9.1.2, dating back to November of 2016. To compare, my docker instance of Owncloud is currently on 10.0.7. 

Plugin jails have been helpful for me and countless others, but it's time to admit that Docker is just better at doing what the original system set out to do. Here's what I've done to transition to Docker on FreeNAS. 

## Setup

Setup is [pretty straight-forward and well-documented in the FreeNAS docs](http://doc.freenas.org/11/vms.html#docker-rancher-vm), however it is described in a very procedural fashion which doesn't really give you any insight of what is being done behind the scene. This makes it very confusing when things break; I was pulling my hair out for hours because of a [corrupt download not completing](https://forums.freenas.org/index.php?threads/freenas-11-1-rancher-server-installation-failed.59898/). For thoroughness' sake, here is an overview of what following the steps in the guide accomplishes:

* Creates a Bhyve VM
* Creates a RAW drive image that will be used for storage
* Downloads the RancherOS image and flashes it (dd?) to your drive image to make it bootable
* Creates a basic RancherOS configuration file from the options entered in the additional options entered in the storage device menu.

If you can't get a serial console or can't get the machine to boot properly, remember this order and use it to diagnose. This should be obvious, but the recommendations regard the provisioning of CPU and RAM resources are just that; depending on what you intend on running within your Docker VM, assign system ressources as necessary. In my case, assigning just one VCPU held back [my bitcoin node](https://hub.docker.com/r/ruimarinho/bitcoin-core/) from properly syncing. Adding another one fixed the problem and made all the services running in containers visibly more snappy. 

A word of warning on giving the VM more system ressources: I had problems giving the machine more than 2 virtual CPUs; the machine seems to want to boot up, with Bhyve init stuff showing up on the serial console, but the OS stalled shortly before all the services could go up. 

## Storage

The modularity of Docker is what makes its strength, but in order to be truly useful, if must be built on a stack that is itself just as modular. For example, in continuity with the spirit of containerizing, I aught to be able to be able to snapshot and replicate individual containers to back them up locally or offsite. Storing everything on the raw disk image just isn't compatible with that vision of things. 

You could store everything on ZFS vdevs through additional VM volume assignments, but then you [lose the ability to snapshot under certain conditions](http://jrs-s.net/2016/06/16/psa-snapshots-are-better-than-zvols/).

Rancher has [convoy](https://github.com/rancher/convoy) that can make docker volumes persistent, and store them on a variety of backends, but the additional layer of docker-style volumes doesn't really need to be there. 

I opted instead to use [nfs-client](https://hub.docker.com/r/d3fk/nfs-client/) and bind-mounts in my containers to my install of FreeNAS, [with the method detailed here](https://forums.freenas.org/index.php?threads/howto-freenas-11-rancheros-docker-and-portainer.54595/). I modified the steps to fit my application slightly. First of all, I elected to created a child dataset for every container, in order to be able to snapshot and replicate as mentionned above. Secondly, I had to make seperate NFS shares for each container, since NFS will not traverse nested datasets as one would expect. With this larger amount of shares, listing the NFS mounts was done directly in my cloud-config.yml file, as so.

![nfsmounts](/content/images/2018/03/nfsmounts.png)

You might be tempted to use SMB, which is also supported by RancherOS; this isn't a good idea if you plan on running any container that includes Apache because it is heavily reliant on UNIX-style permissions to function properly, and that just won't translate over SMB. 

## Networking and security

Using NFS as a link between your Docker VM and your ZFS storage introduces security problems caused by the ways that RancherOS and NFS works. First of all, RancherOS runs all it's Docker processes as root along with the NFS client; secondly, NFSv3 uses client-side info on the user accessing the share to determine if it has the permission to access the share or not. All this means that you will have to disable root squashing for things to work; I went about this by adding root/wheel to the maproot user/group fields in the advanced settings of my NFS shares. 

This is obviously horrible security, because any rogue machine can now access your shares as root if your client ACLs are too loose. The way around this is making sure that the network access to the address that the NFS service is bound to is impossible for everything but your Docker VM. The advanced settings of the individual NFS shares provide a client allow list, which you should use to whitelist ONLY your VM with its IP. 

The ideally you'd also want to use a dedicated network bridge to pass the traffic off between your storage and the VM. The problem is that the web UI does not show you bridge interfaces when offering you the options for VM NIC mapping; my workaround was to plug in a dead-end VLAN in an unused interface on my server, and have all the Docker-related traffic come in through this interface.

### A note on pre-11.1-U2

Going from one reboot to the next in the 11.1 series of releases (not U2 though), I've gotten weird networking issues where the networking interface(s) used by the RancherOS instance would not bind to the correct NIC, making connection to the NAS-provided storage or to the rest of the network impossible. Digging around in the bug tracker, it seems that [the way VM NICs are bridge may be sub-optimal](https://redmine.ixsystems.com/issues/27122). If you are experiencing weird issues after configuring your networking in the RancherOS VM, check your network configuration with `ifconfig`. You should get something like this:

![freenasifbridge](/content/images/2018/03/freenasifbridge.png) 

The TAP interface is the host-side NIC that gives your VM network access. In my setup, the VLAN interface is the parent interface that can be found in the ==Devices/NIC== menu of the VM. If the binding is properly made, the bridge should group those two interfaces together. If you have many TAP interfaces and bridges that you can't make sense of, upgrade to 11.1-U2 and reboot, things should clean up. 

## Managing Docker

RancherOS kind of suggests that using Rancher to manage your containers might be a good idea, but it seemed a bit clumsy and overkill for a single host setup. I instead opted for [Portainer](https://portainer.io), which I was somewhat already familiar with and is much less intimidating to use and deploy, seeing that it's really just a nice front end for the docker commands you you know and love.

At the RancherOS command line, run the installation one-liner on the Portainer website, subsituting bind mounts for the volume if necessary, and adding the restart policy if you so desire. Provided the restart-policy is ok, Portainer will start up with RancherOS bootup. 

## Monitoring

I've been building a monitoring system on the [TICK stack](https://www.influxdata.com/blog/introduction-to-influxdatas-influxdb-and-tick-stack/) for my home setup, and as it turns out Docker monitoring is quite simple. [Telegraf has a plugin specifically for docker monitoring](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker), just spin up a Telegraf instance with the docker.sock endpoint mapped through, configure the config file, and you're good to go.  

With the [intergrated Graphite listener](https://github.com/influxdata/influxdb/blob/master/services/graphite/README.md) enabled and the ports open, you can also configure FreeNAS to output its metrics to Influx aswell... your Influx instance can of course be within docker if you so desire.

If you want better insights into your NAS' performance and don't have a monitoring setup built up, this "FreeNAS monitoring on itself" is not a bad choice, particularly if your FreeNAS server is your only machine.

## Conclusion

If you run anything critical, you might want to run a dedicated docker host. However, if you have spare CPU cycles and memory (although with ZFS, it could be argued that there is no such thing as "spare memory"), why not? Docker absolutely seems like a viable replacement for plugin-jails, and if your FreeNAS box is a backup repo for databases hosted elsewhere, you can now run replication services in containers instead pushing dumps. 

I just wish that Docker was native to the base OS, and that the Bhyve + RancherOS + NFS stack could be avoided. Limits on provisionning hardware are also a bit problematic, but it is what it is. There's a Docker container for everything... have at it!