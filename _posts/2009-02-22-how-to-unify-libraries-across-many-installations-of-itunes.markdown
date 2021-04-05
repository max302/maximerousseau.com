---
layout: post
title: 'How to: Unify Libraries Across Many Installations of iTunes'
date: '2009-02-22 02:24:52'
tags:
- all
- installations
- itunes
- lan
- libraries
- library
- management
- many
- network
- software
- unify
---

About may of last year, I built a server for myself, in hopes of centralizing all the storage drives of my house to one constantly on box and simplifying access to files across the four computers on my network. And sure enough, a couple of hours and hundred dollars later I had myself a kick-arse server rig which housed a RAID1 array for my personal use as well as 3 other random storage drives on which the members of my family could store their music, videos and pictures.

While having my music on a network shares was convient because I could access it from any machine on my LAN without running server software on my main box, I quickly found a flaw in my setup just a couple of months after buying it. Sure, all my music files where save on my server, but the iTunes library XML files were still stored on my local disk, which made re-adding all my music to iTunes after an OS reinstallation unbeleivable long (reading iD3's on 50+ gigs of music isn't what I would call quick), and reading the music from another installation of iTunes on another computer, say a home theatre PC, impossible.

While iTunes allows the user to choose it's "iTunes Music" folder, it does not supply such an option for the library files. Ever since, I've been looking for a way to get my single library to work with many iTunes installations. Sure, there are some how-tos out there, but even the most popular solutions aren't elegant and require third party software. Wouldn't it be great if Windows, <a href="http://homepage.mac.com/paalb/Examples/ln.html">like linux</a>, would allow us to bind symbolic links from one folder to another? Oh wait... there is such a function on Windows Vista. <a href="http://technet.microsoft.com/en-us/library/cc753194.aspx">mklink</a>!

This somewhat obscure command makes everything easier. With mklink, you can simply create a "dummy" iTunes folder in your My Documents folders, and have that folder to point to whatever the heck you want, the latter including network drives. Here's how it's done:
<ul>
	<li>First, you need to create a folder on your remote server that will be taking the place of the iTunes folder where all the XML files and album art is stored. Under XP, this is usually <em>C:\Documents and Settings\Username\My Documents\My Music\iTunes</em> and under Vista it would look something like <em>C:\Users\Username\Music\iTunes</em>. When that's done, transfer what's in your "iTunes" folder to whatever remote path you are using. In my case, my music files (the "iTunes Music" folder) was <em>R:\Music\iTunes\iTunes Music</em>, so as to not mess around too much with the directory structure I decided to create a folder at <em>R:\Music\sync</em> which would act as the target folder.</li>
	<li>Next up, you need to transfer all the stuff in your iTunes folder to the target folder, because you will need to delete the actual iTunes folder in order to create the dummy folder. Once the transfer is done, delete the iTunes folder.</li>
	<li>Lastly, create the dummy folder from the command line using the mklink command. The syntax is pretty simple for this command:
<blockquote>mklink C:\dummy\folder  D:\target\folder</blockquote>
So in my case, the actual command I used was this:
<blockquote>mklink C:\Users\Administrator\Music\iTunes R:\Music\sync</blockquote>
There are many types of links that you can create via this command, but for our us a symbolic link is enough.</li>
</ul>
Once that was done, my <em>C:\Users\Administrator\Music\iTunes</em> folder effectively pointed to <em>R:\Music\sync</em> as intended. iTunes blindly took what XML files were in the destination folder, and loaded my library on first try.

From then on, syncing other instances of iTunes with the current library is a walk in the park: make symbolic links from the iTunes folders of other machines to the same mounted network share, and voila! The other installations of iTunes will read into the library files thinking it is there own, giving you instant access to your music as long as the "iTunes Music" folder path is the same on all the machines. Changes to the library will also be written instantaneously on the unified files, making the changes available on all the other machines.

Before you start unifying your libraries however, there are additional things to take into consideration. Firstly, I would pre-configure the installations of iTunes as I do not know if certain aspects of the configuration (iTunes's Music folder, adding songs to iTunes's Music folder when added to library) are set in the registry or in the XML files. Also, keep in mind that you can not open the same file for read/write operations two times simultaneously, which means that opening two iTunes on the same library at the same time will most probably brick your library files if any changes are made.

Despite it's limitations, this method is the simplest way I found of getting many instances of iTunes to share a same library. No waiting for dropbox or other automated backup software to do it's thing, no maintaining of two sets of library files, just easy access to your music from anywhere on your lan. Obviously, the biggest bummer about mklink is that it only runs under Vista... but hey, if you don't have Vista yet, you should really try it out, and if you don't feel like trying it out, then the very interesting Windows 7 which is right out of bend is your next alternative. Sorry XP users!

Enjoy!