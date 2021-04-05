---
layout: post
title: 'Headless Steam In-Home Streaming, Pt 1: My Experience'
date: '2016-07-28 15:22:25'
tags:
- box
- computer
- games
- gaming
- in-homes-streaming
- rackmount
- remote
- server
- tech
- all
- technology
- steam
---

For almost exactly three years now, I've been using a <a href="https://support.apple.com/kb/sp678?locale=en_US">mid-2013 13.3" Macbook Air</a> as my primary machine. As I explained in a review which has now disappeared with the demise of Epinions, I didn't expect the transition from an expensive gaming rig to a super-slim, barebones laptop to go as smoothly. The idea behind the move was partially to make myself incapable of gaming during University, and partially to have a single machine through which I would use for all my computing needs. I wanted a "single pane of glass", as it were.

Sync solutions that aren't cloud-based generally suck (although <a href="https://owncloud.org">they're</a> <a href="https://getsync.com">getting</a> <a href="https://syncthing.net">better</a>), and even when they don't, there are obvious drawbacks. You know, things like having an internet connection for syncing to happen, or the device needing to be powered up. Controlling several machines in tandem also sucks; <a href="http://symless.com/synergy/">Synergy</a> is a somewhat acceptable solution to doing this, but it's one more piece of software to run, update, configure, and potentially troubleshoot. I <a href="https://maximerousseau.com/2008/01/16/synergy-productivity/">wrote something on this in 2008</a> if you want to cringe. Sometimes, you just want to fire up a machine, open a file you've been working on from your desktop and put in some work, without thinking about it. That's where having one, compact computer with a 10+ hour battery life is very appealing.

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

It's obvious that the Macbook Air has limitations, and getting past those limitations will invariably require more hardware. I was completely kidding myself when I thought I would be making savings by going to a less powerful machine, and I've got several Us of rackspace to show for it. The real challenge isn't really saving money, it's integrating the hardware in a way which sticks to this idea of a single terminal to rule them all. Steam's addition of <a href="http://store.steampowered.com/streaming/">In-Home streaming functionality</a> is great for providing gaming in this way. Here's my experience with getting the thing to run correctly.

<hr />

<strong>My setup</strong>

As with any gaming setup, the sky is the limit as to what you build to use In-Home Streaming. My setup is relatively humble, a far cry from the flashy (and noisy, and expensive) rigs I was building before. Here are the specs.

<strong>Case:</strong> Cooler Master Elite 110
<strong>Power Supply:</strong> Corsair Pro Series AX650
<strong>Motherboard:</strong> Asrock H81-M ITX
<strong>CPU:</strong> Intel Core i7-4790K
<strong>Memory:</strong> Crucial Ballistix Sport 16GB
<strong>GPU:</strong> Zotax GTX 960 2G

This setup lives in a closet along with my other networking stuff, with only a network cable and a power cable going to it.

<hr />

<strong>Common problems and fixes</strong>

The Steam KB covers setup and really common problems well, so I'm not going to bother sharing how to get the thing to work. What I will cover though are the problems that I have personally ran into. Most of them are related to running a machine without any input or output peripherals. Depending on how games manage mouse and keyboard output as well as video output, they may not like not having a physical inputs plugged in, or not being passed on a screen resolution by the OS because the screen is absent.

The most obvious need is for an <strong>emulated display</strong>. Being a <a href="http://folding.extremeoverclocking.com/user_summary.php?s=&amp;u=281903">long-time contributor</a> to the <a href="https://folding.stanford.edu">Folding@Home project</a>, I was well aware that nothing that uses a GPU would work without either a monitor or a dummy plug. The old-school way of doing this is using the DVI-to-VGA plugs that usually come with graphics cards along with some resistors to cook up a <a href="http://www.overclock.net/t/384733/the-30-second-dummy-plug">quick and dirty dummy plug</a>; the folding community has used those for years, and they are known to just work. While the internet specifies 68 to 75 ohm resistors to get this working, I've done mine with 125 ohm resistors to the same effect. The new-school way would be to use an HDMI dummy plug, which has the additional advantage of emulating an audio output, which eliminates the need for more dongles and hacks.

<strong>Audio also needs to have a simulated output</strong>, because most audio chipsets theses days deactivate if no headphones or speakers are plugged into them. While I'm sure we all have spare sets of broken headphones around, neat freaks will likely want a cleaner solution. Most motherboard 3.5mm audio jacks have physical switches inside, so all you need is to physically stuff either <a href="http://www.thingiverse.com/thing:10295">a dummy plug</a> or an actual, unwired 3.5mm plug and onboard sound will work. I've personally run into games which crashes and explicitly gave an audio related message. Some games might work, but you're better safe than sorry.

Finally, <strong>mouse and keyboard input is also need locally</strong> for remote inputs to work properly. I've played through half of AC4: Blag Flag without the mouse's scroll-wheel because using it would cause the game to infinitely scroll. This is reported to happen in several games by several editors on the Steam forums. Here again, having a mouse plugged in is an easy solution, but not quite as clean as one would want it to be. My solution was to use a Logitech Unifying adaptor, without the wireless mouse or keyboard connected, as it registers as both mouse and keyboard HID devices. It's also very low-profile, which is perfect for this application. I've been looking around to find a way of emulating HID devices via software, but I have not yet found anything worth mentioning. From my time in big box stores, I knew for a fact that my local Geek Squad  would have, for no discernable reason, a buttload of orphaned receivers; that's where I got mine. See with your local store if you don't have a receiver handy. Otherwise, the <a href="https://www.amazon.com/gp/product/B01DFPG5CS/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;tag=max302-20&amp;camp=1789&amp;creative=9325&amp;linkCode=as2&amp;creativeASIN=B01DFPG5CS&amp;linkId=5e190445c6c42736a2c385142182c6b2">receivers sell for about 10$ on Amazon</a>.

Once all the inputs and outputs are taken care of, you should have minimized the possibility of games doing weird things on start-up. Then comes the <strong>optimization of the encoding for maximum performance and picture quality</strong>. From the get-go Mac users who have a Nvidia GPU on the backend are SOL when it comes to hardware-accelerated encoding, because of a known incompatibility. The performance is great, but you game will suffer from occasional color spots and artefacts, especially in dark scenes.

<a href="https://maximerousseau.files.wordpress.com/2016/07/screen-shot-2016-07-25-at-2-19-42-pm1.png"><img class="size-large wp-image-1107" src="https://maximerousseau.files.wordpress.com/2016/07/screen-shot-2016-07-25-at-2-19-42-pm1.png?w=480" alt="Examples of artefacts and other problems caused by Nvidia encoding when Apple hardware receives it." width="480" height="141" /></a> Examples of artefacts and other problems caused by Nvidia encoding when Apple hardware receives it.

For my setup, I found that simply deactivating hardware encoding altogether makes for the most beautiful and smoothest gameplay. Intel iGPU mights seem like a good idea, because it offloads to hardware thanks to Intel Quick Sync Video, but I found it to be lacking, with the video output being quite laggy despite framerates staying high. I'm not sure that's what Quick Sync Video was meant for; I'd stay away from it. If you have a modern CPU, chances are your bottleneck isn't the CPU anyhow, so software encoding will probably do little to your experience unless you're running a very CPU-intensive title. Steam forum posts seem to mirror my experience.

<hr />

<strong>Caveats and important considerations</strong>

Despite the fact that this system works great for me, there are some things to be aware of. It works for me because I'm a filthy casual, albeit a very tech-enthousiast filthy casual. I only build a computer to play games when I've amassed too many from <a href="https://www.youtube.com/watch?v=yr2GdRBDOmU">Steam Summer Sales</a> to not play them. I've been gaming on wireless peripherals for a few years now, and I'm not going back. I don't care too much about latencies, refresh rates and the like. If you're a hardcore gamer, you probably hate the idea of streaming a game, because MUH LATENCY. If you're a filthy casual of the non-IT aficionado variety, I'm surprised you're still reading this. This brings me to what is probably the biggest caveat of In-Home Streaming: I feel like it doesn't fit too many use cases yet, for a variety of reasons.

Firstly, <strong>you need fast networking</strong>. I'm talking wired, ideally gigabit if you want your games to run smoothly. Since most normie non-gamers' idea of fast networking is likely router with "Wireless AC" written on it, it's probably not gonna work for most folks. The convenience of wifi has long overtaken good old cables, and it's a hard sell to ask users to drop the convenience and run cables.

Secondly, <strong>it's not like gaming in front an actual display</strong> in terms of imagine quality. The video stream coming from the rendering side of the setup is compressed, and those of you with keen eyes will notice this in certain settings, especially if you're running on a non-gigabit connection.

Thirdly, <strong>it's still not seamless</strong>. Most games will work fine, even the ones that require a launcher or aren't from Steam, but some titles inexplicably require some user intervention via RDP to get things to work. For example, after launching Watch Dogs, other Steam-enabled Ubisoft titles will no longer launch some times, requiring a restart of Uplay. Sticking to the same title is fine, but playing different ones will inevitable at some point cause hiccups which will require user intervention. It's never something major, but it's still a pain in the butt.

Finally, <strong>there is some of the dreaded additional latency,</strong> on input and display. You won't be winning a CS:GO world championship on a stream of a remote computer. It's only really noticeable on faster shooters, but it's there, by the very nature of how it works. As video codecs get better and 10BaseT becomes ubiquitous, we'll see this concern fading away, but for now we have to deal with it.

<hr />

<strong>Conclusion</strong>

To sum it all up, while I don't think the technology is quite mainstream ready, Steam's In-Home Streaming is a step in what I see as the way of the future, that is decoupling a computing/gaming experience from the hardware required to provide it. Let's face it, if you're building your own computer, you probably can handle the additional legwork of configuring and maintaining a streaming box. If on top of that you don't mind making minor concessions on performance for the sake praticality, I'd strongly recommend you look into it. For my, having a totally silent work environment alone is well worth it.

In part 2, I'll go over what I'd like my next Steaming box to look like, and what steps I intend to take to integrate In-Home Streaming functionality to my existing server setup.