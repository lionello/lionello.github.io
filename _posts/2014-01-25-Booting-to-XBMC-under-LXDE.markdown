---
layout: post
title: Booting to XBMC under LXDE
categories: []
permalink: /archives/137-Booting-to-XBMC-under-LXDE.html
s9y_link: http://www.lunesu.com/index.php?/archives/137-Booting-to-XBMC-under-LXDE.html
date: 2014-01-25 19:20:16.000000000 +08:00
---
I recently reinstalled my HTPC using Fedora 19 and, of course, installed <a href="http://xbmc.org/" title="XBox Media Center">XBMC</a> on it.<br />
<br />
Here's how to get Fedora to boot to XBMC without login.<br />
<br />
First, configure your desktop manager to auto-login. In my case that is LXDM and it's configured by editing <strong>/etc/lxdm/lxdm.conf</strong> and adding/changing the line containing <strong>autologin=USER</strong> with <em>USER </em>being the username of the local user you want to run XBMC as. <br />
<br />
Next, change the desktop manager for your user to XBMC. If you install XBMC using the Yum package you should have gotten <strong>/usr/share/xsessions/XBMC.desktop</strong> so it's enough to create a file called .dmrc in the user's home folder.<blockquote>cat >~/.dmrc<br />
[Desktop]<br />
Session=XBMC</blockquote>(End by pressing CTRL-D)<br />
<br />
Finally, to prevent XBMC from using CPU when on the home screen, disable the RSS feed by editing the settings for the default skin. Edit <strong>~/.xbmc/userdata/guisettings.xml</strong> and change <strong>enablerssfeeds </strong>to <strong>false</strong>.<br />
<br />
Reboot to confirm XBMC does indeed start automatically after boot and run <strong>top </strong>remotely to check the CPU usage.<br />
