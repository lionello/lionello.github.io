---
layout: post
title: 'DKIM: DomainKeys identified mail'
categories:
- Knowledge Base
permalink: /archives/101-DKIM-DomainKeys-identified-mail.html
s9y_link: http://www.lunesu.com/index.php?/archives/101-DKIM-DomainKeys-identified-mail.html
date: 2010-03-09 12:53:00.000000000 +08:00
---
In a last effort to save e-mail from being obsoleted, some guys at Yahoo thought of the DomainKeys system to prevent message spoofing. SMTP servers that are DomainKeys-aware can check the contents of the message to find out whether it was really sent by the person mentioned in the From: mail header.<br />
<br />
To be honest, I'm more of a fan of SPF. It's an incredibly simple protocol and I really can't think of any reason it shouldn't be used by any mail server. (Interestingly, the Yahoo mail servers don't use it, probably to boost acceptance of DomainKeys.)<br />
<br />
Let's explain SPF first, since it so much easier. In a nutshell: SPF uses DNS records to identify which IPs are allowed to send mail for a domain. So a server receiving mail for somebody@example.com would fetch the SPF record from example.com and check whether the IP of the connected SMTP client is actually allowed to send mail originating from the example.com domain. <br />
<br />
That's it, it's that easy. Since I'm hosting my own mail server, my domain's SPF record just mentions my mail server's IP (actually, it doesn't mention the IP directly, but refers to its DNS A record.) A server receiving an e-mail that was allegedly sent by me will just have to check whether the SMTP client is in fact my own mail server and if it isn't, reject the mail.<br />
<br />
DomainKeys is a little more advanced. Instead of checking the IP, it will check all contents of the message. It's like the mail server is signing the message on your behalf. Using DKIM, each outgoing message will be signed using the server's private key, and DKIM signatures present in the incoming message will be checked against the originating server's public key. That public key, by they way, is distributed using a DNS record, similar to the way SPF records are distributed.<br />
<br />
Here are the steps I did to get DKIM working on my server. First, create an RSA key pair:<br />
<blockquote>openssl genrsa -out private.key 1024<br />
openssl rsa -in private.key -out public.key -pubout -outform PEM<br />
sudo mkdir /etc/mail<br />
sudo cp private.key /etc/mail/dkim.key<br />
</blockquote>Now you'll have to add the public key to your domain(s) DNS entry. Do this by adding a TXT record for <em>selector._domainkeys.yourdomain.com</em>, where <em>selector </em>can be anything, but make sure to mention the same selector in the config file.<br />
<br />
Next, install dkim-filter and edit the /etc/dkim-filter.conf Domain, KeyFile and Selector settings. Domain= comma separated list of domains for which to sign the mail; KeyFile= path to above keyfile; Selector= domain name prefix.<blockquote>sudo apt-get  install dkim-filter<br />
sudo vi /etc/dkim-filter.conf<br />
sudo /etc/init.d/dkim-filter restart<br />
</blockquote><br />
Add the following to /etc/postfix/main.conf:<br />
<blockquote># DKIM<br />
milter_default_action = accept<br />
milter_protocol = 2<br />
smtpd_milters = inet:localhost:8891<br />
non_smtpd_milters = inet:localhost:8891</blockquote>and restart postfix.<br />
<br />
To test whether it's working as it should, send yourself a mail and inspect the headers. Or, you could for example send a mail to a gmail account and gmail will show that the mail is "signed-by" your server.
