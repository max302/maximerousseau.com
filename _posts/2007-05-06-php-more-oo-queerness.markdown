---
layout: post
title: 'PHP: More OO Queerness'
date: '2007-05-06 16:29:13'
tags:
- all
---

You know what? Forget everthing that I wrote about class variables, I was totally off on what concerned the setting of those variables. Here is the scoop: you cannot, even with $this-&gt;, $this:: or the static keyword prefixing your variables, SET them. I repeat, you can declare a variable but you can not attribute a value to it directly between the classe's curly brackets. In fact, everything that is considered an operation, like incrementing a global which is not even a member to the class, will yeild an unexpected T_VARIABLE error, causing many frustrations for the OO PHP beginner who doesn't know this.

Solutions to this? You'll have to either set your variables using __construct() (not handy for variables that are to be dynamically set), set the vars at runtime outside of the class declaration after creating each instance by using $this and either -&gt; or ::, or creating a function member to your class that takes the task of setting all those variables. Since there is no limitation to what can be done to inside a function, you can manipulate everthing at will, however the idea of calling on a function every time I create an instance is not really appealing.

One solution remains, and that is to hijack a function that would be executed anyways. The idea here is to sneak your value assignments into something like __construct(), which is to be executed at the creation of an instance anyways. __construct() hijacking would look something like this:
<code> class foobar
{
public $lollerskate;  function __construct($c_var1, $c_var2)
{
$this-&gt;var1 = $c_var1;
$this-&gt;var2 = $c_var2;
$this-&gt;lollerskate = $this-&gt;var1 + $this-&gt;var2;
}
}</code>
In this case, __construct() requires arguments to be passed to $c_var1 and $c_var2, but not for lollerskate, adding more flexibility.

Although I have not tested it yet, I am pretty sure that function __construct() can be declared with no arguements, leading it to be run anyways on creation of an instance.