---
layout: post
title: HTTPS, easier than I thought
categories: []
permalink: /archives/74-HTTPS,-easier-than-I-thought.html
s9y_link: http://www.lunesu.com/index.php?/archives/74-HTTPS,-easier-than-I-thought.html
date: 2009-08-08 11:27:36.000000000 +08:00
---
<!-- s9ymdb:61 --><img class="serendipity_image_center" width="241" height="35" style="border: 0px; padding-left: 5px; padding-right: 5px;" src="http://www.lunesu.com/uploads/httpspizzapazzi.PNG" alt="https working on pizzapazzi.com" />

The <a href="http://www.wemin.com/" title="Webmin">Webmin</a> webserver is using a self-signed certificate by default, ie. a certificate that's not authenticated by a (known) Certification Authority. I've been looking up and down for a "certificates for dummies" guide, but all the guides I seem to find are meant for supernatural administrators or something. I was close to giving up and thought I'd just try some brute force approach, by which I mean clicking and copy-pasting until something happens that is close to what I wanted. Maybe it's more like "evolutionary": keep clicking and copy-pasting until it fails, at which point you start over but this time mutating you clicks and pastes. Rinse. Repeat.

Here's what happened. I managed to create a Certificate Signing Request, from Webmin's own private key, using "pizzapazzi.com" as the CommonName:
{% highlight sh %}
openssl req -new -key miniserv.pem -out miniserv.csr
{% endhighlight %}
I pasted that certificate into <a href="https://www.cacert.org/account.php?id=10" title="CACert Request New Server Certificate">CACert.org New Server Certificate</a> page and it spit out some Base64 garbage that I then copy-pasted into a new file on the server. I then selected this new file as the "certificate file" to be used by Webmin SSL.

Lo and behold, after a restart of Webmin I no longer got the usual red HTTPS warning! I have no idea what I did, but it seems to have worked. Now to try something similar for dovecot to get IMAP over SSL.

<strong>UPDATE:</strong> using a private key that you did not yourself create is not secure. Check <a href="http://lunesu.com/index.php?/archives/80-Creating-my-own-certificates.html" title="Creating server keys">this post</a> on how to create your own keys.
