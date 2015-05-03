---
layout: post
title: VPN is working!
categories: []
permalink: /archives/72-VPN-is-working!.html
s9y_link: http://www.lunesu.com/index.php?/archives/72-VPN-is-working!.html
date: 2009-08-07 08:10:33.000000000 +08:00
---
This site is still inaccessible from China, so I decided to get some new hosting with my own dedicated IP. I went with <a href="http://rimuhosting.com">rimuhosting</a>: my own linux VPS at $19/month! Immediately moved all  my websites (except this one) from <a href="http://directnic.com">directnic</a> to rimuhosting. I'll first have to figure out how to do secure IMAP before I move lunesu.com over to the new server.

As you see, I also got PPTP working. Pretty standard really:
{% highlight sh %}
sudo apt-get install pptpd
{% endhighlight %}
Check the entries at the end of pptpd.conf. Make sure the localip and remoteip entries do not conflict with anything else on the network. localip will be the IP of the ppp0 interface on the server. remoteip is the range from which IPs will be handed out to the connecting clients. (*)
{% highlight sh %}
sudo vi etc/pptpd.conf
{% endhighlight %}
We should also route DNS through the VPN. Check the current DNS settings first.
{% highlight sh %}
cat etc/resolv.conf
{% endhighlight %}
Edit the pptpd-options file and enable one or both ms-dns settings, using the IPs from resolv.conf:
{% highlight sh %}
sudo vi etc/ppp/pptpd-options
{% endhighlight %}
Now it's time to add some VPN accounts
{% highlight sh %}
sudo vi etc/ppp/chap-secrets
{% endhighlight %}
One more thing: the server will act as a gateway for the VPN clients and should perform NAT:
{% highlight sh %}
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
{% endhighlight %}
(You will probably want to put that line in some script so it executes automatically after reboot.)
Done! Start or restart pptpd:
{% highlight sh %}
sudo etc/init.d/pptpd restart
{% endhighlight %}

Enjoy!

(* Serendipity does not support blog entries that contain slash-etc-slash! wtf)
