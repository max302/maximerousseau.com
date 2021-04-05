---
layout: post
title: It's One of these Days
date: '2007-04-10 19:46:35'
tags:
- all
---

Welcome, reader, to the new MaximeRousseau.com.

 It`s one of these days again. Or maybe even one of these weeks.  One of these days (or weeks) where nothing seems to be working, where everything fucks up on you. I`m not liking this.

It first started with an unknown crash from a MySQL database. One day, the tables were there, and the other, they were as corrupted as Jean Chrétien’s government. My initial reaction: “Ah, no biggy, I needed to clean my install anyways.” But wait. My host switched servers, and now PHPMyAdmin isn’t installed. Lets get it installed. That’s one week of waiting and poking around with MyAdmin, hence no blogging for me.

When it’s installed, I rush in, backup from the WP util, and then drop all my WP tables. Look back at my site, see everything is up. Again, no reaction from me, after all, maybe it’s just that the SQL server still has a cached version. I come home, log in to messenger, and receive a message from Josh my host. It went amongst these lines:


<blockquote>
FU*******!!! YOU KILLED MY BLOG YOU FU****!</blockquote>



That made me feel really bad. There I go Google myself shitless, trying to save as much of his posts as I possibly can, stuffed that in an archive and emailed that to him telling him that I was really sorry. In then end he didn’t seem to angry about it, except for the loss of data part.

Now, I had to erase MY blog. I go in there, drop all the tables. What do I see? Those god damned corrupted tables that are the source of all this shit. I’m gonna kill these bitches, I’m telling myself. Repair, drop, analyze, nothing works. Another half week messing around with that stuff. In the end, my host fixes it, and I’m back on track. Or am I?

I upgrade to the current version of Wordpress, and start the install process. I load the backup XML that WP had generated. Oh! What a fun surprise! Half of my newer posts, the one that actually made me proud, are gone! Now who do I kill? The moron who coded the WXR generator? No wait calm down, it must be because your other tables were corrupted. Whatever, its all gone and now I have to move on.

Try #2, I install WP once more. I start typing my blogroll, and reload my index, and for an unknown reason, all the themes, even the WP supplied ones, have gone bonkers, with the sidebars now below the list of posts. Somebody reasonable would have scratched his head and troubleshooted the whole thing. But I was far from reasonable, this one simple task of installing Wordpress had taken me about 3 hours, while just a year ago it had taken a total of 20-30 minutes to get it totally customized. Thrash the whole thing, and start all over again.

Now, it works, but I’m friggin fed up of being at my computer, something which I never thought would once happen for something like this. This post is my first one, let it be the only one filled with so much rage. This is a whole new beginning, and this time, I want to make it count. As for the old content, F it, let it rot in hell. I’ll repost if I find it somewhere for some reason, but for the time being, it’s not one of my first priorities.

The moral of this story: Cron is your friend (so is at AT for you windows server users), and a daily backup script can save your ass big time. Dataloss can occur any time, and causes lots of frustration.