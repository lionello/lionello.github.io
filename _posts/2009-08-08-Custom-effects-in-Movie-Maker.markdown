---
layout: post
title: Custom effects in Movie Maker
categories: []
permalink: /archives/77-Custom-effects-in-Movie-Maker.html
s9y_link: http://www.lunesu.com/index.php?/archives/77-Custom-effects-in-Movie-Maker.html
date: 2009-08-08 23:44:35.000000000 +08:00
---
Windows Movie Maker has quite a few tricks up its sleeve. Apparently it's possible to customize the included effects. Do this by placing <a href="http://www.lunesu.com/uploads/effects.xml" title="effects" target="_blank">this file</a> in a newly created folder <em>C:\Program Files\Movie Maker\Shared\AddOnTFX</em>. This will add some custom variants for the standard effects. To change the effects (for example, create an effect that speeds a movie up by 12%) just edit the effects.xml file and copy-paste the appropriate effect (in this example, the speed up effect) and change its parameters.

Of course, using the abovementioned procedure merely creates variants for the standard effects. But it's also possible to <a href="http://msdn.microsoft.com/en-us/library/bb331634(VS.85).aspx" title="Create Movie Maker effects using HLSL">create new effects altogether using HLSL</a>! That, my friends, is pretty cool.
