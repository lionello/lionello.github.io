---
layout: post
title: Google sabotaging Windows Phone clients?
categories: []
permalink: /archives/129-Google-sabotaging-Windows-Phone-clients.html
s9y_link: http://www.lunesu.com/index.php?/archives/129-Google-sabotaging-Windows-Phone-clients.html
date: 2013-01-01 16:05:00.000000000 +08:00
---
(Disclaimer: I'm a Software Engineer for Microsoft China. In this blog I express my personal opinion and none of this constitutes the official nor unofficial opinion of Microsoft.)

<strong>UPDATE:</strong> this issue also hit <a href="http://www.theverge.com/2013/1/4/3836510/windows-phone-8-users-unable-to-access-google-maps">The Verge</a>. The information in <a href="http://www.theverge.com/2013/1/4/3834960/google-maps-disabled-on-wp8">this thread</a> also confirms that Google tests for "Windows Phone", as opposed to testing for specific browser capabilities. <a href="http://gizmodo.com/5973295/google-maps-has-never-been-accessible-on-mobile-internet-explorer">Gizmodo</a> also posted the story.

I make no secret of the fact that, while I like my (Microsoft sponsored) Windows Phone, I miss Google Maps. For me, Google Maps was the killer app for Android. And the experience is great, even in China, with vector maps, buildings, public transport, subway exits, real-time traffic, latitude.

But Google Maps wasn't enough to keep me on Android. I thought the Windows Phone experience was better overall and I ended up using by <strong>Nokia Lumia 800</strong> over my <strong>HTC Desire S</strong>. For maps, I keep switching between Nokia Maps and <a href="http://www.windowsphone.com/en-us/store/app/gmaps-pro/5fad1b41-f8db-4397-b584-4a0da12fe233" title="GMaps Pro">GMaps Pro</a>, an unofficial Google Maps client for WP. And occasionally I'd simply go to <a href="http://maps.google.com" title="Google Maps">maps.google.com</a> with IE. Well, I used to, anyway.

Lately (as of a few months ago) opening maps.google.com on my WinPhone simply redirects me to Google Search. Even the Chinese site, <a href="http://ditu.google.com" title="谷歌地图">ditu.google.com</a>, no longer opens on my WinPhone. Switching IE from Mobile to Desktop mode also doesn't fix the issue, and I keep getting redirected to a regular search page (under the maps.google.com domain, but it's still regular search, without maps.)

My first thought was that the IE browser on WP7.5 "Mango" was simply not supported by Google. So I fired up the WP8 emulator to check whether Google Maps would open on the mobile version of IE10. Sure enough, it didn't. However, this time switching IE to Desktop mode did solve the problem and I got the regular desktop experience, albeit on a small screen.

That's suspicious, I thought, since the browser is exactly the same, whether I use mobile mode or desktop mode. I figure I'd manually try the different <a href="http://en.wikipedia.org/wiki/User_agent" title="User Agent on Wikipedia">User-Agents</a> to see what would happen:

* WP7 - Mobile Version
Mozilla/5.0 (compatible; MSIE 9.0; Windows Phone OS 7.5; Trident/5.0; IEMobile/9.0; NOKIA; Nokia 800)
{% highlight sh %}
HTTP/1.1 302 Found
Location: http://maps.google.com/m/local
{% endhighlight %}

* WP7 - Desktop Version
Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0; XBLWP7; ZuneWP7)
{% highlight sh %}
HTTP/1.1 302 Found
Location: http://maps.google.com/m/local
{% endhighlight %}

* WP8 - Mobile Version
Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; Microsoft; Virtual)
{% highlight sh %}
HTTP/1.1 302 Found
Location: http://maps.google.com/m/local
{% endhighlight %}

* WP8 - Desktop Version
Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Trident/6.0; ARM; Touch; WPDesktop)
{% highlight sh %}
HTTP/1.1 200 OK
{% endhighlight %}

* Win8 - Desktop Browser
Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
{% highlight sh %}
HTTP/1.1 200 OK
{% endhighlight %}

* Win8 - "Metro" Modern Browser
Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0)
{% highlight sh %}
HTTP/1.1 200 OK
{% endhighlight %}

Interestingly, as soon as a device identifies itself as a Windows Phone (or WP7) it gets redirected. In other cases it gets the actual Google Maps page. As a test I wanted to replace the User-Agent from my WP7 device with the one from an Android device. Here's how:
{% highlight sh %}
sudo apt-get privoxy
sudo vi /etc/privoxy/default.actions
{% endhighlight %}
Add this to the end:
{% highlight sh %}
{+hide-user-agent{Mozilla/5.0 (Linux; U; Android 2.3.6; en-us; Nexus One Build/GRK39F) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1)}}
.google.
{% endhighlight %}
Change <strong>listen-address </strong>to listen on a public IP:
{% highlight sh %}
sudo vi /etc/privoxy/config
sudo /etc/init.d/privoxy restart
{% endhighlight %}
On the WP device, change the proxy for your current WLAN profile to point to your instance of <a href="http://www.privoxy.org/" title="Privoxy HTTP proxy">Privoxy</a>.

Et voilà, you got Google Maps on WP7, albeit a little flaky. It might be that Google preferred WP user to have no experience to them having a flaky one, but that still doesn't explain why it's redirecting WP8 IE when in Mobile mode.

Now here's what's interesting. As soon as I add "Windows Phone" to the User-Agent, the page no longer loads and I get redirected again. Clearly there's some regex magic at work on Google's end.

As much as I agree with <a href="http://en.wikipedia.org/wiki/Hanlon%27s_razor" title="Hanlon's razor on Wikipedia">Hanlon's razor</a> "Never attribute to malice that which is adequately explained by stupidity," this does reek of Google being disingenuous and blocking the competition from their platform's killing app.

<a href="http://lunesu.com/index.php?/archives/130-Do-no-evil.html" title=""Do no evil"">And it's not as-if they haven't done shit like this before.</a>

Oh, and Happy New Year!
