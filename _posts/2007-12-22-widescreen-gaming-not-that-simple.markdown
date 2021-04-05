---
layout: post
title: 'Widescreen Gaming: Not That Simple'
date: '2007-12-22 18:38:07'
tags:
- '2'
- all
- battlefield
- battlefield-2
- bf
- enemy-territory
- et
- for
- games
- gaming
- need
- need-for-speed
- nfss
- screen
- software
- speed
- wide
- wolf
- wolfenstein
---

<p align="left">With the purchase of my monitor a few weeks ago and new graphics card just a couple days old, I'm still pretty fresh to the way of life that is using a wide screen monitor. Although with most apps using any resolution is no big deal, as 16:10(monitors) and 16:9 (HDTVs) are supported by Windows natively, but it's really in gaming and web browsing that a wide monitor stands out. Again, on the web the format usually isn't a problem (at least not for the Firefox rendering engine; hurray FF!), but there are some problems in some games. For those trying to play older/crappier games on new monitors, I have set up a small list of games that I personally have had trouble with. Here they are.</p>

<p align="center"><strong> </strong>
<font size="4"><strong>Wolfenstein: Enemy Territory</strong></font>
</p><p align="left">It's a great game, with a gameplay that IMO will outlive Counter Stike, but unfortunately it's already getting pretty old. Just look at the system requirements...</p>

<blockquote>
<p align="left">600 <a href="http://en.wikipedia.org/wiki/Hertz" title="Hertz">MHz</a> <a href="http://en.wikipedia.org/wiki/CPU" title="CPU">CPU</a>, 128 <a href="http://en.wikipedia.org/wiki/Megabyte" title="Megabyte">MB</a> <a href="http://en.wikipedia.org/wiki/Random_access_memory" title="Random access memory">RAM</a>, 32 MB <a href="http://en.wikipedia.org/wiki/OpenGL" title="OpenGL">OpenGL</a> <a href="http://en.wikipedia.org/wiki/Graphics_card" title="Graphics card">graphics card</a>, 56.6k Modem/<a href="http://en.wikipedia.org/wiki/Local_area_network" title="Local area network">LAN</a></p>
<p align="left">--Wikipedia</p>
</blockquote>
<p align="left"> Hrmm. Yeah. Fortunately, the Quake 3 engine being very flexible, there is support for just about resolution you can possibly dream of. Getting it to work correctly is something else. For those who hate fixing up config files, you'll have to endure a slight bit of it: there are 3 and possibly 4 cvars to be changed in the config file. Look for etconfig.cfg in the etmain folder of your Enemy Territory install and fire it up in your favorite text editor.  Find the following cvars with the search function (Control + F).</p>

<blockquote> seta r_mode 3          //Sets the rendering engine to default mode.
seta r_customwidth ""          // Set to nothing
seta r_customheight ""          // Set to nothing</blockquote>
And change them to this :
<blockquote>seta r_mode "-1"
seta r_customwidth "1440"
set r_customheight "900"</blockquote>
In this case, the last two variables reflect my resolution: 1440 x 900, change that to yours. Start the game, and the game should be in the resolution that you have specified. If not, the config decided to not stick. Vista preventing the writing of some files, the game uselessly trying to revert to an old mode, whatever. It's frustrating but there is a way to bypass this. If you used to be a server admin, you might remember creating server start shortcuts and setting some variables on the command line to force the game to load mods from the appropriate folder; well this bypass works the same way. Just add the variables above to your shortcuts in the following form: "et.exe +set r_mode -1 +set r_blablabla..". It's a foolproof solution, just remember to add this to ALL your shortcuts: the Xfire and Rocketdock ones included. Now that you have it running widescreen, you may notice some distortion in the far ends of your screen: mess around with the cg_fov cvar to tweak that. Usually, a 16:10 monitor is gonna work with an FOV of 100.
<font size="4"> </font>
<p align="center"><font size="4"><strong>Battlefield 2 </strong></font></p>
<p align="left">I really don't know what's up with EA, but they don't seem to be digging the whole widescreen thing. Despite the fact that it is WAY more recent than ET, it still doesn't support widescreen without some hacking, minor hacking that is. Still, that doesn't make it any more acceptable. With the unreal amount of patches that EA release, they could of at least thought into making 16:10 an option. Meh... Well anyways, here is how to force it to take your resolution. It's kind of like ET, only there is no configuration files to be edited, it only works with command line variables. Just add the following line to your command line arguments on your shortcuts:</p>

<blockquote>
<p align="left"> +menu 1 +fullscreen 1 +szx 1440 +szy 900</p>
</blockquote>
<p align="left">+Menu and +fullscreen are important, but I forget why, while szx and szy values are to be replaced with your desire resolution. No need to tweak FOV, the game looks fine with no additional tweaking.</p>
<p align="center"><font size="4"><strong>NFS: Carbon</strong></font></p>
<p align="left">Despite the fact that this game is still relatively recent, it STILL doesn't support widescreen. And guess who's the publisher? That's right! EA! This time, the process of putting it 16:10 is a tad tougher. First, you'll need a no CD fixed EXE. Not only will this liberate you from EA's annoying tendency to keep asking for a CD in hope of saving themselves from piracy, which is totally retarded because nobody uses optical drives anymore,  but it is also a mandatory part of the widescreen patching process. Because of possible legal issues, I can't give you the crack right here right now, but I can point you to Gamecopyworld, which is the best site I know for files of the kind. Look for a working crack, and don't forget to always backup your old executable and scan all shady files for viruses. Once you got the cracked game working, you can go ahead and download the <a href="http://www.widescreengamingforum.com/wiki/index.php?title=Downloads#Universal_Widescreen_.28UniWS.29_Patcher">UniWS patcher from here</a>, and <a href="http://www.widescreengamingforum.com/forum/viewtopic.php?p=70341">follow these instructions to patch your exe</a>. Once that is done, your game should be working properly. If you're like me and you use Xfire, you might want to change back the game exe to NFSC.exe after you have patched it, anything else will not be detected by Xfire, hence no game hour logging, no in-game chat, etc, etc. Of course, the same process applies to Need For Speed Underground 2, since the patch was originally created for that game.</p>
<p align="left">Although this isn't a big list, they are the one's that I have found up to now. If you're looking for more wide screen gaming resources, check out <a href="http://www.widescreengamingforum.com/wiki/index.php/Main_Page">WSGF</a>, which looks like THE big resource on the thing. Have fun.</p>