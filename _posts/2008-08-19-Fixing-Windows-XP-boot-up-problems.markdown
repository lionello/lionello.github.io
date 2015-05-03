---
layout: post
title: Fixing Windows XP boot-up problems
categories: []
permalink: /archives/48-Fixing-Windows-XP-boot-up-problems.html
s9y_link: http://www.lunesu.com/index.php?/archives/48-Fixing-Windows-XP-boot-up-problems.html
date: 2008-08-19 22:31:14.000000000 +08:00
---
Ine had some laptop problems and I've spent the last evening of my holiday trying to fix them. Things I've learned, in chronological order:

* <a href="http://www.ubuntu.com/getubuntu/download" title="Download Ubuntu">Ubuntu live CD</a> rulez: I was able to find corrupted files using <em>diff * --ignore-file-name-case --to-file=dllcache/</em>

* Copying <em>ntoskrnl.exe</em> from <em>dllcache </em>to <em>system32 </em>fixed the <a href="http://en.wikipedia.org/wiki/Blue_Screen_of_Death" title="Blue Screen Of Death">BSOD</a> by causing the laptop to lock up instead

* Recovery Console sucks: it kept prompting for an administrator password that I didn't know

* Recovery Console really sucks: its <em>chkdsk</em> reports errors, but does not fix them

* During the graphical part of Windows XP Setup, you can open a console window by pressing Shift-F10

* Reset the administrator password using <em>net user administrator *</em>

* System Restore rulez: using plain old copy I was able to <a href="http://support.microsoft.com/kb/307545" title="How to recover registry">restore the registry from the recovery console</a>. Simply cd into <em>C:\System Volume Information\</em> and copy <em>_REGISTRY_MACHINE_X</em> to <em>\windows\system32\config\X</em>

* System Restore UI (<em>\windows\system32\restore\rstrui.exe</em>) sucks: it restored all personal settings to the original defaults!

* Windows XP Repair sucks: repair would fail because "the signature for windows xp home edition upgrade is invalid"

* <em>sfc /scannow</em> is nice and all, but kept <a href="http://support.microsoft.com/kb/897128/" title="Bug in XP Home">prompting for the Windows XP Professional CD when run on Home Edition</a> (Same problem as previous one?)

* Using the Windows XP <a href="http://support.microsoft.com/kb/943144" title="Windows Repair breaks Windows Defender">Repair function will break Windows Defender's auto-update</a>

