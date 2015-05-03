---
layout: post
title: HTTPS on apache, trickier than I thought...
categories: []
permalink: /archives/78-HTTPS-on-apache,-trickier-than-I-thought....html
s9y_link: http://www.lunesu.com/index.php?/archives/78-HTTPS-on-apache,-trickier-than-I-thought....html
date: 2009-08-09 13:53:53.000000000 +08:00
---
HTTPS for Webmin is pretty easy, see yesterday's post, but getting SSL to work with Apache took a couple of hours. Here are the steps and pitfalls.<br />
<br />
First, enable the "ssl" module. I'm using Webmin, so in Webmin's Apache modules, go to "Global configuration", then select "Configure Apache modules" and check the "ssl" module. After you "apply changes" a new module called "SSL Options" will appear in Webmin.<br />
<br />
Apache should listen on port 443 as well. In "Global configuration", go to "Networking and addresses". In the "Listen on addresses and ports" table, switch the last row from "None" to "All" and enter "443" for port. Apply changes.<br />
<br />
In Apache, we need a separate irtual host to handle the HTTPS request on port 443. (This is different from Microsoft's IIS where one virtual host can handle both HTTP and HTTPS traffic.) The existing virtual hosts probably listen on all ports so now it's time to limit those hosts to port 80. Select the existing host and in "Virtual server details" enter port "80" and click the radio button to the right of it.<br />
<br />
Now we create a new virtual host for HTTPS. Go to the "Create virtual host" tab. Change the port to "443" and check the radio button to the left of it. I chose the copy all directives from the existing virtual host, so if you want to do the same, select the existing host in "Copy directives from" drop down box. Save and apply changes. The new virtual host should appear in the server list. Select the new host and go to its "SSL Options" page. Check "Enable SSL" and select the certificate and the private key to use for SSL. I've chosen the same certificate and private key as the one used by Webmin. If you've combined the certificate and private key into a single file (by simple concatenation) you only need to set one option. Otherwise, set both options. Apply changes.<br />
<br />
As far as Webmin's concerned, we're done. But unfortunately it still didn't work for me. The following errors appeared in /var/log/apache2/error.log:<br />
<blockquote>[error] VirtualHost *:80 -- mixing * ports and non-* ports with a NameVirtualHost address is not supported, proceeding with undefined results<br />
[error] VirtualHost *:443 -- mixing * ports and non-* ports with a NameVirtualHost address is not supported, proceeding with undefined results</blockquote><br />
<br />
Apparently the error is caused by the NameVirtualHost directive being out of sync with the VirtualHost directive in apache's configuration files. I fixed this error by editing etc/apache2/sites-available/default and commenting out the "NameVirtualHost *" line (add a # before it):<br />
<blockquote>sudo vi etc/apache2/sites-available/default<br />
sudo etc/init.d/apache2 restart<br />
</blockquote><br />
<br />
After restarting Apache the error.log no longer showed the aforementioned error messages and HTTP and HTTPS were both working.
