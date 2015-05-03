---
layout: post
title: VPN is working!
categories: []
permalink: /archives/72-VPN-is-working!.html
s9y_link: http://www.lunesu.com/index.php?/archives/72-VPN-is-working!.html
date: 2009-08-07 08:10:33.000000000 +08:00
---
This site is still inaccessible from China, so I decided to get some new hosting with my own dedicated IP. I went with <a href="http://rimuhosting.com">rimuhosting</a>: my own linux VPS at $19/month! Immediately moved all  my websites (except this one) from <a href="http://directnic.com">directnic</a> to rimuhosting. I'll first have to figure out how to do secure IMAP before I move lunesu.com over to the new server.<br />
<br />
As you see, I also got PPTP working. Pretty standard really:<br />
<blockquote>sudo apt-get install pptpd</blockquote><br />
Check the entries at the end of pptpd.conf. Make sure the localip and remoteip entries do not conflict with anything else on the network. localip will be the IP of the ppp0 interface on the server. remoteip is the range from which IPs will be handed out to the connecting clients. (*)<br />
<blockquote>sudo vi etc/pptpd.conf</blockquote><br />
We should also route DNS through the VPN. Check the current DNS settings first.<br />
<blockquote>cat etc/resolv.conf</blockquote><br />
Edit the pptpd-options file and enable one or both ms-dns settings, using the IPs from resolv.conf:<br />
<blockquote>sudo vi etc/ppp/pptpd-options</blockquote><br />
Now it's time to add some VPN accounts<br />
<blockquote>sudo vi etc/ppp/chap-secrets</blockquote><br />
One more thing: the server will act as a gateway for the VPN clients and should perform NAT:<br />
<blockquote>sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE</blockquote><br />
(You will probably want to put that line in some script so it executes automatically after reboot.)<br />
Done! Start or restart pptpd:<br />
<blockquote>sudo etc/init.d/pptpd restart</blockquote><br />
<br />
Enjoy!<br />
<br />
(* Serendipity does not support blog entries that contain slash-etc-slash! wtf)
