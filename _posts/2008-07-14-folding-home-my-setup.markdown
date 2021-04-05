---
layout: post
title: 'Folding @ Home: My Setup'
date: '2008-07-14 16:26:46'
tags:
- all
- at
- distributed-computing
- fah
- folding
- home
- medical
- research
- stanford
---

In the last weeks, I've spent lots of time <a href="http://www.overclock.net/blogs/max302/531-fah-road-top-100-thanks-smp.html">optimizing my folding farm</a> and thinking about <a href="http://www.dreamincode.net/forums/showtopic54352.htm">future upgrades</a>, and I thought I might show off my setup to the public, and show how easy and fun it can be to start folding and cure them diseases.

As illustrated in the wonderful graphic below, my folding setup is composed of mainly 5 elements.
<p style="text-align:center;"><a title="FAHsetupblog by Maxime Rousseau, on Flickr" href="http://www.flickr.com/photos/maximerousseau/2552498628/"><img class="aligncenter" src="http://farm4.static.flickr.com/3119/2552498628_5c279f21dd_o.jpg" alt="FAHsetupblog" width="520" height="328" /></a></p>
<p style="text-align:left;">Let's go through the elements one by one.</p>

<ol>
	<li><strong>1. </strong>This is Annabel, my fastest folder. The particularity of this machine is that somehow, despite that it is not the most powerful machine in the house, it's the one churning out the most points. Despite 400 less mhz, half the L2 cache and slower memory, it manages to outperform Fr0stbyte, the strongest machine in my household, by about 25%. Could it be that it's running Linux? My guess is that the OS probably does play a huge role: At equal clocks, we could estimate that Linux SMP would probably yield 30-40% more points than Windows SMP.Annabel runs Linux SMP very close to 24/7, it's only downtimes being reboots and component swaps. She also serves as an HTTP, FTP, SSH, Bittorrent server and NAS node, serving 2 entire hard drives as well as 3-4 other folders on the network via SMB. She runs on Fedora 9 "Sulphur" x64.</li>
	<li><strong>2.</strong> My first ever computer, Fr0stbyte, is the second biggest folder in the house. It's got the most elaborate cooling too, so it's overclocked O' plenty: the e6750 which runs stock at 2.66 mhz was given a healthy ~800 mhz boost and now runs at 3.4 ghz, all day, everyday, or almost. Fr0stbyte runs on Windows XP Pro x64, and folds with the Windows Nvidia GPU2 folding client. It used to do SMP, but at about 5000 PPD, the GPU client yeilds much more.</li>
	<li><strong>3.</strong> Not much to say about this box... I've been folding on this machine ever since I started folding, but at less than 200 PPD it's pretty much useless. It's been folding a tad more since I swapped the 1.7 ghz Celeron for a 2.2 ghz Pentium 4, but it's still one of the second slowest folder around my house.</li>
	<li><strong>4.</strong> Last one is my father's laptop. DO NOT FOLD ON A LAPTOP UNLESS IT IS COOLED CORRECTLY! This one happens to be a 17 incher, and the temps stay ok, which is the only reason while I still have a client running on it. It runs only a couple of hours per day and it sports a single core Turion, so yeild is minimal.</li>
	<li><strong>5.</strong> My pride and joy in this whole setup is my monitoring setup. It all starts with Fr0stbyte running Fahmon, the folding at home log reader and monitor. I just run Fahmon on my main box because I had some problems solving depencies for installing it in linux, but I could very well have run it on any of my other boxes.  Fahmon happens to have an "export for web" option which generates an HTML page every time it updates, for use with a web server to display stats for the world to see. Since I have my web server on a network share on my main rig, I just configure Fahmon to shoot it's web stuff to the web server's network drive mount, and voila. If I see that a box is considered idle by fahmon, then I SSH to my server for some hot pinging action, and I reboot (or call a relative and tell him/her to reboot) the box in need.</li>
</ol>
Sure, I've seen people with way sicker farms, but I AM at rank 162 of the OCN folding team, so mine can't possibly be that bad. Remember, you don't need monster machines to get folding for the cure. Check out my other articles on folding, (<a href="http://www.overclock.net/blogs/max302/369-folding-linux-short-how.html">How to fold on linux</a> and a short rambling on <a href="http://www.overclock.net/blogs/max302/574-smp-gpu-numbers-they-re-growing.html">how to optimize your setup</a>), and put those spare cycles to use.