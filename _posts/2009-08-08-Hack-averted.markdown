---
layout: post
title: Hack averted
categories: []
permalink: /archives/73-Hack-averted.html
s9y_link: http://www.lunesu.com/index.php?/archives/73-Hack-averted.html
date: 2009-08-08 10:33:09.000000000 +08:00
---
I still have to get used to my new role as a linux server administrator.

I tried to login to my server the other day but accidentally mistyped the server address as "pizzapazzi.zom". Of course, putty warned me that the host's key was not known and whether I wanted to add it, but I clicked 'yes' before I was even consciously aware of the warning dialog. Login failed, of course, but I kept trying. After trying to login using my personal user account, I desperately tried to login as root, which failed as well.

Now, what if those *.zom people have an automated script which tries to login silently to *.com and immediately execute a passwd to change the password? They'd have to delete any public keys from .ssh as well. I'm glad they were not that nefarious, although after I realized my typo I quickly reset all my passwords... This does sound like an easy way to hack servers though. Register <em>commondomain</em>.zom (or any other top level domain with a <a href="http://en.wikipedia.org/wiki/Hamming_distance" title="Hamming distance @ wikipedia">hamming distance</a> of 1 to "com"), host a fake SSH service, possibly proxying the real server's banner. To prevent the user from realizing the mistake they should proxy the whole session and allow the user to login to the actually (proxied) shell account.

Lesson learned. Time to disable password login altogether I guess.

(To revoke hostkeys from putty, check HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\SshHostKeys)
