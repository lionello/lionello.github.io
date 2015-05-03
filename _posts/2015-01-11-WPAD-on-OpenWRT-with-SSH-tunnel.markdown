---
layout: post
title: WPAD on OpenWRT with SSH tunnel
categories: []
permalink: /archives/146-WPAD-on-OpenWRT-with-SSH-tunnel.html
s9y_link: http://www.lunesu.com/index.php?/archives/146-WPAD-on-OpenWRT-with-SSH-tunnel.html
date: 2015-01-11 23:20:43.000000000 +08:00
---
<ol><li>Create your <a href="http://en.wikipedia.org/wiki/Proxy_auto-config" title="Proxy auto-config">PAC</a> file, for example<blockquote>function FindProxyForURL(url, host) {<br />
  if (shExpMatch(host, "*.google.com") || host == "google.com") {<br />
   return "PROXY wpad:8888";<br />
  }<br />
  else {<br />
    return "DIRECT";<br />
  }<br />
} </blockquote></li><br />
<li>SCP the PAC file to <strong>root@openwrt:/www/wpad.dat</strong></li><br />
<li>Go to Luci -> Network -> Hostnames and add <strong>wpad</strong> as an alias for your OpenWRT router's IP</li><br />
<li>Install the <strong>autossh</strong> software package through <strong>opkg</strong></li><br />
<li>SSH to the router and edit <strong>/etc/config/autossh</strong><blockquote>config autossh<br />
  option ssh	'-2 -N -o ServerAliveInterval=60 -L 192.168.1.1:8888:127.0.0.1:8888 -i /root/.ssh/id_rsa user@example.com'<br />
  option gatetime	'0'<br />
  option monitorport	'0'<br />
  option poll	'600'<br />
</blockquote></li><br />
<li>Use <strong>dropbearkey</strong> to create an SSH keypair, save the private key in <strong>/root/.ssh/id_rsa</strong></li><br />
<li>Append the newly created public key to your server's <strong>~/.ssh/authorized_keys</strong></li><br />
<li><strong>Enable</strong> and <strong>start</strong> the autossh service through Luci</li><br />
</ol><br />
You can test your setup with curl:<blockquote>curl wpad/wpad.dat<br />
curl --proxy wpad:8888 google.com</blockquote><br />
<br />
This will have created an SSH tunnel from your router to your remote server, reconnecting as soon as a disconnect happens. Clients on your network will route traffic through the proxy, as determined by your logic in the PAC file.<br />
Windows clients use <a href="http://en.wikipedia.org/wiki/Web_Proxy_Autodiscovery_Protocol" title="Web Proxy Autodiscovery Protocol">WPAD</a> by default, but for OSX clients this has to be explicitly enabled in Network Settings.
