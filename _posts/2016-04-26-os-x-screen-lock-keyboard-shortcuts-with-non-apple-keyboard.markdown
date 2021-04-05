---
layout: post
title: OS X Screen Lock Keyboard Shortcuts with Non-Apple Keyboard
date: '2016-04-26 19:49:24'
tags:
- air
- all
- apple
- automator
- filco
- haswell
- keyboard
- mac
- macbook
- magic-trackpad
- majestouch
- minila
- monitor
- os-x
- service
- shortcut
- sleep
---

For 3 years now, I have been using a <a href="http://www.amazon.com/gp/product/B00746YPQI/ref=as_li_tl?ie=UTF8&amp;camp=1789&amp;creative=9325&amp;creativeASIN=B00746YPQI&amp;linkCode=as2&amp;tag=max302-20&amp;linkId=ABAOLJ6VQEN5IQ4I">base mid-2013 Haswell-powered Macbook Air</a> as my primary machine. At the time, I was just entering university, and I sold my all-custom, high-dollar gaming rig to keep myself away from gaming, and in the hopes of stopping my excessive hardware-buying habit. That failed miserably, and I now run about 20Us of server gear to do what my Macbook Air cannot do. Regardless, I am still enjoying my Macbook as a "single pane of glass"-ish interface for all my computing needs: whether I'm on the go or at my desk, my desktop remains the same. Microsoft has notably been wanting to get customers to sync their systems through clouds services with recent additions to Windows, but in my limited experience, it just doesn't work too well.

My use-case means that most of the time, I'm using my computer at a desk, with an external monitor, keyboard, and trackpad. While this is great in terms of ergonomics and productivity, it also means that normal keyboard shortcuts available on the Apple keyboard are not available, because my laptop is always tucked away with the cover closed. My setup is comprised the Air, a <a href="http://www.amazon.com/gp/product/B00NAWCU7G/ref=as_li_tl?ie=UTF8&amp;camp=1789&amp;creative=9325&amp;creativeASIN=B00NAWCU7G&amp;linkCode=as2&amp;tag=max302-20&amp;linkId=JWHT36HZTGXFZIOD">Belkin Thunderbolt dock</a>, a <a href="http://www.amazon.com/gp/product/B00F3V81VG/ref=as_li_tl?ie=UTF8&amp;camp=1789&amp;creative=9325&amp;creativeASIN=B00F3V81VG&amp;linkCode=as2&amp;tag=max302-20&amp;linkId=PPDOHJQG2CXJWBOD">Filco Majestouch Minila Air bluetooth mechanical keyboard</a>, a 24 inch Samsung monitor, and an <a href="http://www.amazon.com/gp/product/B016QO5YWC/ref=as_li_tl?ie=UTF8&amp;camp=1789&amp;creative=9325&amp;creativeASIN=B016QO5YWC&amp;linkCode=as2&amp;tag=max302-20&amp;linkId=HA7M4SDOEMFHVKIJ">Apple Magic Trackpad</a>.

<a href="http://maximerousseau.com/2016/04/26/os-x-screen-lock-keyboard-shortcuts-with-non-apple-keyboard/photo-2016-04-26-14-59-48_1477/" rel="attachment wp-att-1022"><img class="aligncenter size-large wp-image-1022" src="https://maximerousseau.files.wordpress.com/2016/04/photo-2016-04-26-14-59-48_1477.jpg?w=480" alt="Photo-2016-04-26-14-59-48_1477" width="480" height="360" /></a>

For safety, energy savings and distraction avoidance, I like to periodically put my monitors to sleep while working on something else at my desk, or when leaving it. The built-in Windows shortcut Win+L is something I used a lot on Windows for that very reason. OS X has a similar, very well documented keyboard shortcut CMD+SHIFT+EJECT or CMD+SHIFT+POWER depending on what kind of Mac and keyboard you are using which does exactly the same thing. Achieving this shortcut on a non-Apple keyboard requires a bit of a work-around.

If you're familiar with Windows, you'd be tempted to make a batch script that puts the displays to sleep through a command line, make a shortcut of that batch script somewhere, then assign a keyboard short to that said shortcut. This cannot be done in OS X. The process instead has two steps:
<ol>
 	<li>Creating a "service" that exists in the background for the sole purpose of running our commands when it is invoked.</li>
 	<li>Creating the actual keyboard shortcut.</li>
</ol>
The first part is done with through Automator. Launch the application, selection <strong>New Document</strong> when prompted, then  select <strong>Service</strong> as the type of document. This will bring you to the interface where we will insert the task to be executed. In our this, this is a simple terminal command, so add the <strong>Utilities &gt; Run Shell Script</strong> action, and get your service looking something like this.

<a href="http://maximerousseau.com/2016/04/26/os-x-screen-lock-keyboard-shortcuts-with-non-apple-keyboard/automator/" rel="attachment wp-att-1020"><img class="aligncenter size-large wp-image-1020" src="https://maximerousseau.files.wordpress.com/2016/04/automator.png?w=480" alt="automator" width="480" height="327" /></a>

Be careful to set the <strong>Service receives</strong> setting to <em>no input. </em>I've checked the <strong>Ignore this action's input<em> </em></strong>because there really shouldn't be any interaction with the service, either on input or output; once it's called upon, we want this action to execute and nothing else. Save it with an appropriate name, and that's all that has to be done in Automator.

The next step is to go to <strong>System Preferences &gt; Keyboard &gt; Shortcuts</strong><em>.</em> What we want here is a shortcut to a service, so the obvious option is to select <strong>Services</strong> on the right of the window. Your new service will be found at the very bottom of the list of services, under <em>General.</em> From here, uncheck the service if it is checked, then click to the right of the service where you should have a subdued <em>None</em>. Enter your keyboard combo, and the service should be checked once it is set. I chose CTRL+CMD+SHIFT+S, since it is unlikely to be activated by mistake, used by another application, and relatively easily activate with a single hand.

<a href="http://maximerousseau.com/2016/04/26/os-x-screen-lock-keyboard-shortcuts-with-non-apple-keyboard/shortcut/" rel="attachment wp-att-1021"><img class="aligncenter size-large wp-image-1021" src="https://maximerousseau.files.wordpress.com/2016/04/shortcut.png?w=480" alt="shortcut" width="480" height="418" /></a>

That's it. As soon as it is activated in the System Preferences, your shortcut is active.

The command we've entered in our service puts the monitor to sleep, but this action is still considered by the system as a more  general "sleep" state. This means that you can control password prompting from the usual spot in <strong>System Preferences &gt; Security &amp; Privacy &gt; General </strong>with the Require password option. Personally, I ask for password every time, because I'm really only using this new shortcut when I'm away from the machine. It takes a few more seconds to log in on wakeup, but I just write that off as an additional way of a distraction elimination; I'll be much less tempted to aimlessly browse Facebook if there is a barrier to browsing in the form of a password prompt.

There is one caveat, being that some applications seem to catch keyboard interrupts without relaying them to OS X, hence blocking certain keyboard actions. In my case, having Chrome as an open app in the foreground prevents the shortcut from executing. I have not been able to find another app in which this is a problem; all the Apple preloaded software does not seem to be affected by this. CMD-TAB'ing out of whatever it is you're using to Finder in order to run the shortcut is not too much a hassle, so I'm still pretty satisfied.

Automator is pretty powerful; as somewhat of a former Windows power user, I'm kind of surprised that I didn't get around to scripting with it. Calendar alarms provide very cron-like functionality, but things like folder workflows can potentially make file sorting and repetitive manipulation tasks much easier. The interface is much more friendly than hand-crafting commands in a traditional shell script for the average mortal. Cool stuff.