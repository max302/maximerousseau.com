---
layout: post
title: Bypassing Bell Fibe FTTH Hub with Ubiquiti EdgeMAX Equipment
date: '2016-08-01 02:05:06'
tags:
- '100'
- '1000'
- '2000'
- all
- bell
- edgerouter
- fibe
- fiber
- hub
- internet
- isp
- networking
- pppoe
- sagemcom
- tech
- ubiquiti
- ubnt
---

Whenever I have to interact with big telcos, I inevitably come to ask myself why they are still in business. It's a wonder that companies that are so big and so dysfunctional on so many levels still have any customers at all. I've recently had to do an ISP switchover from dual Cogeco 100mbps over copper to a single Bell Fibe 250mbps line, and my experience was less than stellar. Apart from getting the usual "oh, we're sorry, your line isn't quite active yet on our end" not once, but twice after the install tech's visit, their business technical support was entirely useless.

<center><a href="https://maximerousseau.files.wordpress.com/2016/07/fst4350.jpg"><img class="size-full wp-image-1119" src="https://maximerousseau.files.wordpress.com/2016/07/fst4350.jpg" alt="Sagemcom FAST4350" width="299" height="337" /></a> 

The dreaded Sagemcom FAST4350 aka Bell Hub 1000. Bell can connect to this to diagnose stuff apparently; you know what they say about back doors... Good riddance.</center>


If you're a Fibe customer, you probably know that Bell insists that you use their Hub 1000 or 2000 (their name for it, it's actually a Sagemcom FAST4350 router) in your networking setup, regardless of the fact that as a business customer, you probably have something more suitable for the job. If you try to bypass it, then don't hope to get any kind of technical support: the mindless automatons that they haven't managed to outsource or replace with machines at their level one support will absolutely refuse to escalate your call or provide any sort of useful information. The Sagemcom has a bridge modem that can be triggered with button presses: if it's bridging, they also won't be supporting it. I've heard they have remote access to the router many times from many sources, and I would be tempted to believe it. Since I or my customer wasn't about to let a 30$ piece of shit router with a back door be the weak link in our connectivity, we found a way to get it working with the Edgerouter we had on site, while completely bypassing the Sagemcom box.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-9587657333914786"
     data-ad-slot="9140171587"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<center><a href="https://maximerousseau.files.wordpress.com/2016/07/img_8614.jpg"><img class="size-large wp-image-1118" src="https://maximerousseau.files.wordpress.com/2016/07/img_8614.jpg?w=480" alt="aa" width="480" height="360" /></a>

The Alcatel-Lucent ONT. This is obviously not optional.</center>

The <a href="http://blog.ngpixel.com/post/104449747538/how-to-bypass-bell-fibe-hub-and-use-your-own-router">VLAN 35 / VLAN 36 trick is well known</a>, albeit completely undocumented by Bell, and seems to work network-wide. Basically, traffic to the Bell ONT is divided into two VLANs, 35 for the internet traffic and 36 for the Fibe TV streams. Our use case only had an internet connection, so getting it to work was as simple as creating a VLAN interface on ethernet interface we used to connect to the Alcatel Lucent ONT, and creating a PPPOE interfaces within this VLAN. Once that's done, enter your username and password for the PPP connection as provided by Bell, set the firewall rules for your connection and you're good to go. The commands should look like this:
<blockquote><strong>set interfaces ethernet ethX vif 35</strong>
<em>     #creates VLAN 35 on interfaces ethX</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0</strong>
<em>     #creates PPPOE connection zero within the newly created VLAN</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0 username thisisyourusername@bellnet.ca</strong>
<em>     #self-explanatory</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0 password thisisyourpassword</strong>
<em>     #ditto</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0 firewall in name WAN_IN</strong>
<em>     #set firewall inbound firewall rules</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0 firewall local name WAN_LOCAL</strong>
<em>     #set firewall local rules</em>
<strong>set interfaces ethernet ethX vif 35 pppoe 0 default-route auto</strong>
<em>     #get your routes from your PPPOE connection</em></blockquote>
By default, your Edgerouter uses all the right options for connection to PPPOE properly, except for one. MTU was set to 1492 explicitly without my intervention, and name-server to auto also. This will allow you to connect and get an IP, however only certain parts of the internet will be accessible. I thought this had something to do with routes, but both a static interface default route and the PPPOE routes yielded the same results. Connection to Youtube and other Google sites worked great, but half the internet didn't, with packets stopping 6 or 7 gateways down. Turns out MSS clamping is to blame, so you'll need to set that in your firewall.
<blockquote><strong>set firewall options mss-clamp mss 1412</strong></blockquote>
Without this option, your packets can end up being too large and dropped by certain hops which enforce maximum MTU strictly. What's funny is that MSS clamping is assumed if you use the wizards to define your WAN as PPPOE, but not one of the assumed options when setting up PPPOE from scratch. Since we were migrating from a load-balanced setup with DHCP being provided by modems, this was not present in our config.

Keep in mind that this will work for setups without Fibe TV only. For setups with it, <a href="http://blog.ngpixel.com/post/104449747538/how-to-bypass-bell-fibe-hub-and-use-your-own-router">you can follow the excellent instructions from here</a>, with the added bonus that you can probably use <a href="https://help.ubnt.com/hc/en-us/articles/204952244-EdgeMAX-How-do-I-bridge-eth2-3-and-4-switch0-with-eth1-on-the-EdgeRouter-POE-">software port bridging functionality</a> in your Edgerouter to eliminate the need for a switch between your ONT and the Bell Hub. This is entirely untested though, so have fun at your own risk.

My primary frustration with Bell in this case, let's say apart from having to go on-site three times to get a relatively simple job done, is that I'm pretty certain a more senior level 2 technician would have pointed to the MSS stuff in a matter of minutes; it's just the kind of stuff you know when you work in that stuff all day. I know for a fact that there are some technically competent folks who work at or for Bell, because I've had great experiences in the past. I've assisted in some pretty hard-core roll-outs of Bell services to entreprise customers, and the techs there were present and helpful. Where they were not, a quick chat with the sales guy  could fix things very rapidly. The problem is that if you don't have a sales guy, even if you're a business customer, you have to navigate through the same byzantine system of call-in support as the rest of the plebs, wait a long time and hope for the best. If you're lucky you'll get a zealous one who's willing to break SOP to make you happy; if you're unlucky, you'll get another drone who just reads the lines he's supposed to.