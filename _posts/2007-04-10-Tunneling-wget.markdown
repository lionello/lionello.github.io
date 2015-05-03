---
layout: post
title: Tunneling wget
categories:
- Knowledge Base
permalink: /archives/20-Tunneling-wget.html
s9y_link: http://www.lunesu.com/index.php?/archives/20-Tunneling-wget.html
date: 2007-04-10 14:02:00.000000000 +08:00
---
It's too bad wget does not support a SOCKS proxy. To tunnel wget through an SSH tunnel you'll have to rely on a statically mapped port:<br />
```
plink -L 8080:example.com:80 server
```
<br />
This command opens a local TCP port 8080 and routes all connections to <em>example.com</em> port 80, using an SSH connection to <em>server</em>. Here, <em>server</em> can be either  the URL or IP of an SSH server or the name of a putty profile. <br />
<br />
wget can now be pointed to the <em>localhost:8080</em>, but this won't work if the HTTP server is using virtual hosts. Use the <em>--header=</em> option to insert a fake host string in the HTTP request:<br />
```
wget --header="Host: example.com" http://localhost:8080/.....
```
