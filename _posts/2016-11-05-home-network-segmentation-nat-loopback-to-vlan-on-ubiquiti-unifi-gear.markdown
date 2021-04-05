---
layout: post
title: Home Network Segmentation, NAT Loopback to VLAN on Ubiquiti Unifi Gear
date: '2016-11-05 02:30:15'
tags:
- all
- loopback
- network
- security
- segmentation
- ubiquiti
- ubnt
- unifi
- vlan
---

I've been putting off segmenting my network for a while now, but the recent IoT botnet powered DDoS has bumped the task up my list of priorities, and I finally got around to doing it. Generally, if your network is anything other than non-critical clients accessing the internet, that is to say if you have any sort of IoT devices or it you host any internet-facing services at home, it's probably a smart thing to split up your network into segments. Doing so allows finer-grained control over which machines can talk to each other, thus enhancing security. A segmented network is usually also easier to survey and audit, because irregularities like "why the hell is there an Acer laptop in my server segment?" stand out more, and with the appropriate monitoring solutions you can more easily generate usage stats by just running queries for an entire segment.

Let's use the IoT botnet situation as a practical example. If your chinese-made, off-brand IP camera system was rooted, we now know that it has the potential to take part in taking down a third of the internet. But being that a third party has complete control over the device, there's no telling what what it could do, including spread to other machines on your network, steal some files off an improperly secured network share, etc. Not being able to shitpost to Facebook is one thing, but messing around with my self-hosted stuff and data is where I draw the line.

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

There exists several models for how to segment networks, but this is one of those things that is really an exercise in common sense. A military buddy once evoked a three tier system that's the standard for military IP networks; that's probably great for their use case, but your home isn't a forward operating base in the horn of Africa, and the military probably doesn't have the likes of Nests and Augusts in their networks. Thanks to the magic of VLANs, you aren't bound to a specific number of segments, and you're probably better off doing more than not enough, within reason. Here's what my setup looks like. <a href="https://maximerousseau.files.wordpress.com/2016/11/apptwifi.png">
</a>

<center><a href="https://maximerousseau.files.wordpress.com/2016/11/homenet.png"><img class="aligncenter size-large wp-image-1143" src="https://maximerousseau.files.wordpress.com/2016/11/homenet.png?w=480" alt="homenet" width="480" height="216" /></a></center>

It's a pretty simple setup, but since the pictures are not really representative, here's some more explanation.

<span style="text-decoration:underline;"><strong>General access:</strong></span> This is the untagged network that anybody who connects via wifi or wired connection gets assigned to without further configuration. This is the least trusted segment, in that it has limited access to the others, but it's also protected from the other segments.

<span style="text-decoration:underline;"><strong>Web-facing services:</strong></span> Self-explanatory. This is completely isolated from all the other segments, both in and out, because clients who normally just connect through WAN. There is an exception for local clients trying to connect from <strong>General access</strong> and <strong>Network managment</strong> segments, which get NAT'd back in, but those exceptions are made for application specific ports only and everything else can be firewalled out. For Unifi gear, everything needs to talk back to the controller for your network to work, and if that controller is local, you'll need to punch a few holes in your firewall. More on this later.

<span style="text-decoration:underline;"><strong>Network management:</strong></span> This is for the administration interfaces of switches, routers and access points, in addition to IPMI and other remote KVM options. In a Unifi environment, nobody should be talking to the APs or the switch, so can normally be isolation. A per-host exceptions can be made if you want to access those devices, most likely an IPMI console, with your main machine. Seeing that IPMI practically gives the next best thing to physical access to your systems and <a href="https://community.rapid7.com/community/metasploit/blog/2013/07/02/a-penetration-testers-guide-to-ipmi">has been proven to be a wide-open opportunity for attack</a>, it stands to reason to reason that this segment should be as strongly sealed up through your firewall policy. Note that for now, Unifi APs do not support being managed on a tagged VLAN, although the feature is <a href="https://community.ubnt.com/t5/UniFi-Feature-Requests/Allow-management-traffic-to-be-tagged-to-specific-VLAN/idc-p/591285#M108">supposed to be in the works</a>. My own Unifi switch did not have this problem with the latest firmware.

<strong><span style="text-decoration:underline;">Home Automation:</span></strong> Since home automation products <a href="https://maximerousseau.com/2016/10/18/the-internet-of-things-needs-integrators-not-gadgets/">usually only talk to the internet anyways</a>, this is another locked-down segment that can't speak to anybody. Where necessary, an appliance that needs access to more "commercial-grade" products that actually allow interaction from the LAN can be assigned an address within the VLAN. While it's not deployed right now, a virtualized machine running <a href="https://home-assistant.io">Home Assistant</a> would be interesting.

<hr />


Building a segmented network with a Unifi gateway as your router is a bit different from what could be done on other platforms, since the incomplete GUI controls don't offer all the options necessary to fine-tuning your setup. The major annoyance is that NAT loopback (aka hairpin or reflection) doesn't seem to be properly implemented. Port-forwarding via the USG configuration menu works when accessing from the internet, but loopback config seems to assume that you will only be forwarding ports to a single subnet, and hence only need loopback to and from this subnet. We'll need to fix this.

<center><a href="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-3-06-58-pm.png"><img class="size-full wp-image-1146" src="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-3-06-58-pm.png" alt="Unifi USG port forward dialog. Simple enough." width="375" height="343" /></a></center>

I found the fix for this while reading <a href="https://help.ubnt.com/hc/en-us/articles/204952134-EdgeMAX-NAT-Hairpin-Nat-Inside-to-Inside-Loopback-Reflection-">a date how-to for EdgeMAX products</a>. Once you've configured your port forwards, you'll have to manually setup NAT masquerading to the hosts that will be receiving the forwards for loopback to function correctly. There are no means of settings these things up in the GUI, so you'll have to use <a href="https://help.ubnt.com/hc/en-us/articles/215458888-UniFi-How-to-further-customize-USG-configuration-with-config-gateway-json">the old config.gateway.json trick to manually input</a> some NAT rules.

I strongly recommend learning how to program Edgemax / Unifi gear, because it's great help in understanding how to modify the config file, but in case you're in a hurry, here's a sample of the JSON you'll need to add.

<pre><code> 
{
     &quot;service&quot;: {
          &quot;nat&quot;: {
               &quot;rule&quot;: {
                    &quot;7001&quot;: {
                         &quot;description&quot;: &quot;Hairpin NAT Transmission&quot;,
                         &quot;destination&quot;: {
                               &quot;address&quot;: &quot;10.1.2.3&quot;,
                               &quot;port&quot;: &quot;9091&quot;
                         },
                         &quot;log&quot;: &quot;disable&quot;,
                         &quot;outbound-interface&quot;: &quot;eth1.121&quot;,
                         &quot;protocol&quot;: &quot;tcp&quot;,
                         &quot;source&quot;: {
                               &quot;address&quot;: &quot;10.1.0.0/24&quot;
                         },
                        &quot;type&quot;: &quot;masquerade&quot;
                    }
               }
          }
     }
}</code></pre>

Replace the description, destination address and port, source address range, outbound interface (mind the VLAN!) and you are good to go. You will need one rule for every host. This JSON was built assuming you have nothing in your on-controller config file, do respect the hierarchy if you have other things going on in there, and <a href="http://jsonlint.com">ALWAYS validate your code</a>.

You'll need a rule for every port forward you want accessible from your LAN. It might be possible to define those in one shot by defining several destination addresses and ports, but I have't tested this.

In the event that the destination port on your forward is not identical to the incoming port, you'll want to configure the inside port in this masquerade for it to work. Be careful, loopback traffic is not exempt from passing through your LAN-IN firewall rules, so you'll need to configure exemptions to let that through.

<center><a href="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-10-16-35-pm.png"><img class="size-large wp-image-1160" src="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-10-16-35-pm.png?w=480" alt="This is what my LAN-IN firewall config looks like. Note the exemptions: 1) for NAT loopbacks, 2) for the switch on the management segment, which still needs to talk to the controller which has not yet been migrated from my general access segment. Rule 2003 is disabled for the same reason." width="480" height="212" /></a></center>

This is what my LAN-IN firewall config looks like. Note the exemptions: 1) for NAT loopbacks, 2) for packets coming back on active connections to services available through loopback connections and 3) for the switch on the management segment, which still needs to talk to the controller which has not yet been migrated from my general access segment. Rule 2004 is disabled for the same reason.
<center><a href="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-3-46-48-pm.png"><img class="size-large wp-image-1153" src="https://maximerousseau.files.wordpress.com/2016/11/screen-shot-2016-11-04-at-3-46-48-pm.png?w=480" alt="The content of Web-Service-Loopbacks group. Note that I used hosts, not the entire segment, which would negate my isolation rules. " width="480" height="145" /></a></center>

The content of Web-Service-Loopbacks group. Note that I used hosts, not the entire segment, which would negate my isolation rules.

That's it! While you're doing the legwork of editing the JSON file, you might want to do things like disable SSH password authentication...

<pre><code> 
{
     &quot;service&quot;: {
          &quot;ssh&quot;: {
               &quot;disable-password-authentication&quot;: &quot;''&quot;
          }
     }
}</code></pre>

...and configure RSA login.

<pre><code> 
{
     &quot;system&quot;: {
          &quot;login&quot;: {
               &quot;user&quot;: {
                    &quot;admin&quot;: {
                         &quot;authentication&quot;: {
                               &quot;public-keys&quot;: {
                                    &quot;user@hostname&quot;: {
                                         &quot;key&quot;: &quot;aaaaabbbbbcccccddddd&quot;,
                                         &quot;type&quot;: &quot;ssh-rsa&quot;
                                    }
                               }
                         }
                    }
               }
          }
     }
}</code></pre>

That's a pretty good start to a robust and secure home network. Let me know if I missed anything.