---
layout: post
title: HTTP proxy through SOCKS
categories: []
permalink: /archives/82-HTTP-proxy-through-SOCKS.html
s9y_link: http://www.lunesu.com/index.php?/archives/82-HTTP-proxy-through-SOCKS.html
date: 2009-08-20 10:07:05.000000000 +08:00
---
One of the easiest ways, I find, to use a proxy is to use <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/" title="PuTTY">Putty</a> to create a local SOCKS proxy and configure the browser or application(*) to use this SOCKS proxy. Unfortunately, many applications don't support SOCKS but do support HTTP proxies. (wget is one of those applications that don't support SOCKS but unlike many others <a href="http://lunesu.com/index.php?/archives/20-Tunneling-wget.html" title="Tunneling wget">wget can still use an SSH tunnel</a>.) But there's a trick we can use from the <a href="http://www.torproject.org/" title="Tor-Project">Tor-Project</a>: use Privoxy. <br />
<br />
<a href="http://www.privoxy.org/" title="Privoxy">Privoxy</a> is a small HTTP proxy that can optionally route all traffic to another HTTP or, and here's the thing, SOCKS proxy. After the installation of Privoxy there's just one line to add to its main configuration file, config.txt:<br />
```
forward-socks4   /               127.0.0.1:9050		.
```
<br />
This will tell Privoxy to route all traffic through the SOCKS server at localhost port 9050. The port 9050 is the port you configured in Putty's dynamic routing tab or after the -D command line option in Plink. By default, Privoxy listens on localhost:8118 so reconfigure your program to use that as a HTTP proxy and you're set. Note that you will have to restart Privoxy after making a change to its configuration file.<br />
<br />
* or the old FileZilla 2.x; the new one does not support SOCKS and the old one is nowhere to be found <img src="http://www.lunesu.com/templates/default/img/emoticons/sad.png" alt=":-(" style="display: inline; vertical-align: bottom;" class="emoticon" />
