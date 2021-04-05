---
layout: post
title: 'OO PHP: Working with Class Variables'
date: '2007-04-30 17:42:04'
tags:
- all
---

<blockquote> WARNING! I currently still at my noob stage in OO PHP, and in OO in general. Do not take anything found in this article as rock solid content that you can trust, for all I know, even the slightest bit of code that I pasted may have errors in them, as I generally learn from error messages. Feel free to point the baddies out, and they will be corrected.</blockquote>
It's been only a few weeks since my first venture in OO PHP (which for some weird reason looks pretty obscure), and already I have found something that is very sparsely documented, or at least not very easy to find for the common noob.

My project was a simple generator that gives off some network information, which you might use if you read an article later on that I'm currently cooking up. It uses groups and machines, and each group gets declared by the user, and a soon as a group is spawned, a user can add a machines to it. In my C person mind, the way to go was 2 and even 3 dimensional arrays to put the groups and machines in, as well as the information relative to them. A group variable call would then have looked something like this:
<code>$group[1]['name'];</code>
But this simply lacked elegance, so I just back off the project for while, thinking that it wasn't top priority anyways.

In the meanwhile, I picked up that Syngress book on C# that I had won on Sploitcast. For those not familiar, C# is a hybrid language between Java and C++, bringing the object aspect of Java into the coolness that is C++. It's .NET flavor is a very interesting option for easy GUI (OO) programming under windows. Like Java, it uses lots of classes and methods and members andall of that OOP goodstuff. Being a straightforward C person for the most part, and having never even thought about using OO, the concepts that you could actually create your own class that contain whatever you wanted were very hard to grasp, and since I just had done only good ol' functions and arrays programming before, I could hardly think up a use for these funky practices.

But then, reading a chapter in my book about how every GUI element of C#.NET use lots of member methods and variables as properties and actions, the light shone upon me, and the magic solution came to me.  What if each group was declared as and individual element in an array, then its properties accessible like elements in other classes. What if it looked like this?
<code>$group[1]::$name;</code>
Hey, it's not that different, but it works, and it's OO. Now, no more messing around with multidimensional arrays that are hard to manipulate, now, a new instance of my class can be created, facilitating manipulation. Here is how it works.

It's pretty simple really. Create a class by declaring one with the "class" keyword. It's basically like create a function.
<code>class MyClass
{class stuff goes here!}</code>
This hasn't created a class yet, but has just created the template amongst which an instance will. If this still doesn't tell you anything, this analogy may help you: a class is like a mould, amongst which you can create a vase, an instance. The mould determines what can be put in the class. The class can be declared to contain anything that a normal PHP app can contain, might it be functions or variables, which later are available from external classes through the -&gt; or :: symbol, the scope resolution operator. I personally prefer ::, simply because it can reference objects that have not yet been created, whereas -&gt; will give off an error if calling an inexistent class. Calling stuff then looks like this:
<code>$class::$stupidvar;
$otherclass::stupidfunction();</code>
The only problem with classes is that it makes variable ambiguity more present, making place for more precise variable scope declarations, unfound in simpler PHP. You might have check the PHP manual for keywords such as "static" and "public", and other scope variable documents, but in the end, setting those becomes natural.

Now for my actual problem with OOP variables: the simple task of putting a variable in a class. In other languages, declaring a variable in a class is as simple as declaring a variable elsewhere. However, in OO PHP just giving something like "static var $foobar" will yield an unexpected variable error.  On <a href="http://dreamincode.net">dreamincode</a>, I was quickly pointed to __construct(), a function that more explicitly declares the variables to be used in a class. This function is very sparsely documented for some reason. It looks something like this:
<code>class foo
{
function __construct($const_othervar)
{$variable = $const_othervar;}
}</code>
In this example, $const_othercar is just a buffer to be used to initial the variable properly. When __construct() is inside a class, it can be called at the same time as the creation of a new instance, like the following:
<code>$classinstance = new class($whatever);</code>
As many variables as you want can be created from __construct(), with the same format. This assures a faultless variable initialization within a class. After proper initialization, the variable in the class instance with -&gt; or ::, in the form of $classcontainer::$variable.

So, after all that messing around, what are the advantages? Not very much, from what I read, to the exception of increased readability and understandability, not to mention easier spawning of frequently used items, which is the goal of OO. Still to me, the why of OO is somewhat obscure, but nonetheless I encourage you to continue exploring OOP: after all, is what everybody is coding right now, why shouldn't you do it?