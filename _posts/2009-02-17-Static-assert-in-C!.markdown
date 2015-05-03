---
layout: post
title: Static assert in C#!
categories:
- Knowledge Base
permalink: /archives/62-Static-assert-in-C!.html
s9y_link: http://www.lunesu.com/index.php?/archives/62-Static-assert-in-C!.html
date: 2009-02-17 16:48:27.000000000 +08:00
---
{% highlight sh %}
partial class StaticAssert
{
	byte a = RenderQuality.Low < RenderQuality.Medium ? 0 : -1;
	byte b = RenderQuality.Medium < RenderQuality.High ? 0 : -1;
}
{% endhighlight %}

Apparently ?: is being folded at compile time, resulting in the following error if the constant expression before it evaluates to false:
{% highlight sh %}
Constant value '-1' cannot be converted to a 'byte'
{% endhighlight %}

I use partial class to prevent collisions with multiple static asserts in the same namespace, although you will still have to come up with unique variable names.

Also, I've hidden my post about [tee'bet] to see if that will lift the blockade of my site in China.
