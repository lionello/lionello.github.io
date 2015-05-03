---
layout: post
title: WPAD on OpenWRT with SSH tunnel
categories: []
permalink: /archives/146-WPAD-on-OpenWRT-with-SSH-tunnel.html
s9y_link: http://www.lunesu.com/index.php?/archives/146-WPAD-on-OpenWRT-with-SSH-tunnel.html
date: 2015-01-11 23:20:43.000000000 +08:00
---

1. Create your <a href="http://en.wikipedia.org/wiki/Proxy_auto-config" title="Proxy auto-config">PAC</a> file, for example
{% highlight sh %}
function FindProxyForURL(url, host) {
  if (shExpMatch(host, "*.google.com") || host == "google.com") {
   return "PROXY wpad:8888";
  }
  else {
    return "DIRECT";
  }
}
{% endhighlight %}


1. SCP the PAC file to <strong>root@openwrt:/www/wpad.dat</strong>

1. Go to Luci -> Network -> Hostnames and add <strong>wpad</strong> as an alias for your OpenWRT router's IP

1. Install the <strong>autossh</strong> software package through <strong>opkg</strong>

1. SSH to the router and edit <strong>/etc/config/autossh</strong>
{% highlight sh %}
config autossh
  option ssh	'-2 -N -o ServerAliveInterval=60 -L 192.168.1.1:8888:127.0.0.1:8888 -i /root/.ssh/id_rsa user@example.com'
  option gatetime	'0'
  option monitorport	'0'
  option poll	'600'
{% endhighlight %}


1. Use <strong>dropbearkey</strong> to create an SSH keypair, save the private key in <strong>/root/.ssh/id_rsa</strong>

1. Append the newly created public key to your server's <strong>~/.ssh/authorized_keys</strong>

1. <strong>Enable</strong> and <strong>start</strong> the autossh service through Luci

You can test your setup with curl:
{% highlight sh %}
curl wpad/wpad.dat
curl --proxy wpad:8888 google.com
{% endhighlight %}

This will have created an SSH tunnel from your router to your remote server, reconnecting as soon as a disconnect happens. Clients on your network will route traffic through the proxy, as determined by your logic in the PAC file.
Windows clients use <a href="http://en.wikipedia.org/wiki/Web_Proxy_Autodiscovery_Protocol" title="Web Proxy Autodiscovery Protocol">WPAD</a> by default, but for OSX clients this has to be explicitly enabled in Network Settings.
