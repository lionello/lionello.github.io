---
layout: post
title: Swap Command and Ctrl for Windows on Mac
categories: []
permalink: /archives/139-Swap-Command-and-Ctrl-for-Windows-on-Mac.html
s9y_link: http://www.lunesu.com/index.php?/archives/139-Swap-Command-and-Ctrl-for-Windows-on-Mac.html
date: 2014-06-19 11:06:36.000000000 +08:00
---
Quick <a href="http://www.lunesu.com/swapctrlcmd.reg">registry hack</a> that turns the Mac Command keys into Ctrl keys and turns the left most Ctrl key into the Windows key:<blockquote>Windows Registry Editor Version 5.00<br />
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]<br />
"Scancode Map"=hex:00,00,00,00,00,00,00,00,04,00,00,00,1D,00,5B,E0,1D,E0,5C,E0,5B,E0,1D,00,00,00,00,00</blockquote><br />
The format of the Scancode Map is as follows: 8 bytes header (all 0x00), 4 byte integer "count", followed by "count" times scancode pairs, with each pair consisting of 2 16-bit scancodes with "to" followed by the "from" scancode.<br />
<br />
0x0000 will disable the key. The last pair is always 0x0000 0x0000.<br />
<br />
You'll need to restart Windows in order for the map to apply.
