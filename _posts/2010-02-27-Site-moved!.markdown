---
layout: post
title: Site moved!
categories:
- Knowledge Base
permalink: /archives/100-Site-moved!.html
s9y_link: http://www.lunesu.com/index.php?/archives/100-Site-moved!.html
date: 2010-02-27 18:25:22.000000000 +08:00
---
I've moved this blog from <a href="http://directnic.com" title="DirectNic">directnic.com</a> to my own VPS. This blog is using <a href="http://www.s9y.org/" title="Serendipity Blog Software">Serendipity</a>, which consists of a bunch of files for the webserver and a MySQL database. (I during the move I noticed that the new version supports SQLite backend, which would have made this whole process a lot easier.)<br />
<br />
What I did:<br />
<ul><li/>Copy all files from the old server to the new server, using <a href="http://www.ncftp.com/" title="NcFTP">ncftp</a>'s recursive mget.<br />
<li/>Dump the MySQL database using <em>mysqldump</em><br />
<li/>In MySQL client: create a new database using <em>CREATE DATABASE xxx</em>, select it using <em>use xxx</em> and execute the exported SQL script using <em>source dumpfile</em><br />
<li/>Configure an Apache virtual server for the lunesu.com domain. Note to self: must check <em>Include all addresses</em> in Webmin's <em>Networking and Addresses</em> Apache configuration tab.<br />
<li/>Apply the new Apache configuration.<br />
<li/>Add lunesu.com to my local <em>hosts</em> file, to be able to test the new site without having to change the DNS settings.<br />
<li/>Oops: ncftp did not get the <em>serendipity_config_local.inc.php</em> because it was readable by user httpd only! The only way I was able to copy this file was by uploading a dummy PHP script with the following contents: <em>&lt;? header('Content-Type: text/plain' ); readfile("serendipity_config_local.inc.php");</em><br />
<li/><em>UPDATE serendipityConfig SET value="/var/bla/path" where name="serendipityPath";</em><br />
<li/>I also had to change some folder permissions: <em>chmod 1777 . archives upload plugins templates_c</em><br />
</ul><br />
The serendipity Weather sidebar plugin failed to work. I read that it needs the PEAR:Services-and-Weather module for PHP so I installed that using <em>sudo apt-get install php-services-weather</em>. Unfortunately, that completely borked my site. I was able to track this down to a function in Serendipity called get_plugin_title. Apparently, even with that module installed, the weather plugin still wouldn't work. Fix: disable the weather plugin.
