---
layout: post
title: Static assert in C#!
categories:
- Knowledge Base
permalink: /archives/62-Static-assert-in-C!.html
s9y_link: http://www.lunesu.com/index.php?/archives/62-Static-assert-in-C!.html
date: 2009-02-17 16:48:27.000000000 +08:00
---
<blockquote><br />
partial class StaticAssert<br />
{<br />
	byte a = RenderQuality.Low < RenderQuality.Medium ? 0 : -1;<br />
	byte b = RenderQuality.Medium < RenderQuality.High ? 0 : -1;<br />
}<br />
</blockquote><br />
<br />
Apparently ?: is being folded at compile time, resulting in the following error if the constant expression before it evaluates to false:<br />
<blockquote>Constant value '-1' cannot be converted to a 'byte'</blockquote><br />
<br />
I use partial class to prevent collisions with multiple static asserts in the same namespace, although you will still have to come up with unique variable names.<br />
<br />
Also, I've hidden my post about [tee'bet] to see if that will lift the blockade of my site in China.
