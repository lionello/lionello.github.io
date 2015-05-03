---
layout: post
title: Shouting Winamp to Icecast
categories: []
permalink: /archives/121-Shouting-Winamp-to-Icecast.html
s9y_link: http://www.lunesu.com/index.php?/archives/121-Shouting-Winamp-to-Icecast.html
date: 2011-02-13 15:49:43.000000000 +08:00
---
Did I mention how great the Squeezebox Boom sounds? Well, it sounds great.

I have bought Shukar Collective's latest album, <a href="http://www.amazon.com/gp/product/B000W3N7A0/ref=dm_att_alb5" title="Shukar Collective - Rromatek">Rromatek</a>, on Amazon today. (Amazon MP3s can only be bought from the US; nothing a VPN can't solve.) Once the download finished I listened to the CD on my laptop, but my Dell has terrible speakers. I immediately wished I could listen to the CD from the Squeezebox instead.

The solution I came up with was to hook my Winamp up to the Icecast server on my NAS. Turns out that's harder than you'd think. There's a Winamp DSP plugin that does just that (<a href="http://www.oddsock.org/" title="Oddcast">Oddcast</a> aka Edcast), but it was abandoned, apparently because of "legal reasons".

Luckily, Icecast now accepts Shoutcast clients, but the protocol used by Icecast is not the same as the one from Shoutcast and the Shoutcast Source Plugin (get it <a href="http://www.shoutcast.com/download" title="Shoutcast Radio Tools">here</a>) doesn't understand Icecast. Fortunately, since version 2.2, Icecast can handle Shoutcast sources, by adding a single line to your &lt;listen-socket&gt; section in icecast.xml:
{% highlight sh %}
&lt;shoutcast-mount&gt;/strm&lt;shoutcast-mount&gt;
{% endhighlight %}
I wanted a new mount for my Winamp, not to interfere with the one from Ices, so it became:
{% highlight xml %}
    &lt;listen-socket&gt;
        &lt;port>8000&lt;/port&gt;
        &lt;shoutcast-mount&gt;/winamp&lt;/shoutcast-mount&gt;
    &lt;/listen-socket&gt;
    &lt;mount&gt;
        &lt;mount-name&gt;/winamp&lt;/mount-name&gt;
    &lt;/mount&gt;
{% endhighlight %}
In Winamp, go to Preferences, Plugins, DSP/Effects and configure the SHOUTcast Source DSP. Check "Use SHOUTcast v1 mode (for legacy servers)". Disable "Make this server public" on the Yellowpages tab. Select an audio format on the Encoder tab. Press connect, and we're done.
