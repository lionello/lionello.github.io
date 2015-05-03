---
layout: post
title: LinkStation playing music!
categories:
- Knowledge Base
permalink: /archives/104-LinkStation-playing-music!.html
s9y_link: http://www.lunesu.com/index.php?/archives/104-LinkStation-playing-music!.html
date: 2010-03-19 14:02:40.000000000 +08:00
---
Inspired by this page on <a href="http://buffalo.nas-central.org/wiki/Add_a_USB_sound_card" title="Add a USB sound card">NAS-Central</a>, I ordered a <a href="http://item.taobao.com/item_detail.jhtml?item_id=50334a3c57df67f334cf4d7b70b6d67c&x_id=db1" title="USB 5.1声卡">USB sound card on TaoBao</a> (15 RMB including delivery!) and plugged it into my <a href="http://lunesu.com/index.php?/archives/96-Heh.html" title="Open stock LinkStation">LinkStation</a>. A quick <em>dmesg</em> showed that the device was detected, but not recognized.<br />
<br />
The NAS-Central page mentions that some kernel modules are needed to get USB audio to work, but of course, the device does not have these modules by default. After googling for a few hours I still could not find any precompiled modules that matched my LinkStation's kernel version, 2.6.22. I decided to jump in and try to compile the modules myself.<br />
<br />
So how do you compile kernel modules? <br />
<br />
Well, first of all you'll need the correct kernel sources, but that's easy: <a href="http://opensource.buffalo.jp/gpl_storage.html" title="Buffalo NAS sources">Buffalo provides them</a>. Download the <em>linux-*</em> package that matches your NAS.<br />
<br />
Now you need a compiler. Turns out Buffalo uses CodeSourcery so we best stick to that as well. Get the pre-compiled cross-compilation toolchain <a href="http://www.codesourcery.com/sgpp/lite/arm/releases/2005q3-2" title="CodeSourcery cross-compilation toolchain">here</a>. Select "ARM GNU/linux" and the operating system that you will use for building. No need to download the toolchain source.<br />
<br />
Before we can compile the modules we need a kernel configuration file. Some Buffalo source packages seem to include it (the file is called <em>.config</em> and is hidden because of the leading dot) but the one I downloaded did not include one. Copy this file into the root of the folder where you've extracted the Buffalo sources. This .config file most likely has all support for audio disabled so we'll enable it by adding <em>CONFIG_SND=m</em> (or check whether there's a <em>CONFIG_SND</em> present and change that to <em>=m</em>). The 'm' tells the make file to build it as a separately loadable module, instead of building it into the kernel. (Since I didn't want to change the stock kernel, I had to build loadable modules instead.)<br />
<br />
Now the good part starts: <em>make modules</em><br />
<br />
The make file should notice the new CONFIG_SND directive and ask you about the specific sound related modules. Make sure you answer 'm' to all of 'em. You don't need them all, but figuring which you need/don't need is too much work.<br />
<br />
When make is done we need to 'install' the modules, but not in our host/build system of course! Use the INSTALL_MOD_PATH directive to copy the built modules into a separate folder: <em>make modules_install INSTALL_MOD_PATH=/tmp/MyModules</em><br />
<br />
When done, copy all the files from /tmp/MyModules/lib/modules/2.6.22.7ownkernel/kernel/sound (recursive) to your NAS. Now comes another the tricky part: loading the needed modules. The module you <strong>need</strong> is <em>snd-usb-audio</em> and <em>snd-pcm-oss</em>, but before this one will need you'll have to load a couple of others. The normal way this is done is to move the modules into the NAS's kernel folder and issue a <em>depmod -a</em> to update the module dependecies and a <em>modprobe snd-usb-audio</em> to load the module with its dependencies. I did it the wrong way: using <em>insmod</em> to load each module by hand, and trying to figure out the order in which to load them by trial-and-error. Eventually, <em>insmod snd-usb-audio.ko</em> loaded without complaints.<br />
<br />
Last step: create the device nodes. As per the NAS-Central page:
```
cd /dev<br />
addgroup audio<br />
mknod -m 660 mixer c 14 0 ; chgrp audio mixer<br />
mknod -m 660 mixer1 c 14 16 ; chgrp audio mixer1<br />
mknod -m 660 dsp c 14 3 ; chgrp audio dsp<br />
mknod -m 660 dsp1 c 14 19 ; chgrp audio dsp1<br />
```
<br />
I am now able to stream music using mpg123, madplay, mpd (all installable using ipkg.)<br />
<br />
The sound quality of that 15 RMB sound card sucks though <img src="http://www.lunesu.com/templates/default/img/emoticons/sad.png" alt=":-(" style="display: inline; vertical-align: bottom;" class="emoticon" /><br />
<br />
Here's the final <a href="http://www.lunesu.com/uploads/config.bz2" title="config.bz2" target="_blank">.config</a> file I used to build the sound modules.
