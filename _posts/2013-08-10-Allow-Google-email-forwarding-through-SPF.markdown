---
layout: post
title: Allow Google email forwarding through SPF
categories:
- Knowledge Base
permalink: /archives/134-Allow-Google-email-forwarding-through-SPF.html
s9y_link: http://www.lunesu.com/index.php?/archives/134-Allow-Google-email-forwarding-through-SPF.html
date: 2013-08-10 16:34:33.000000000 +08:00
---
Use "include:aspmx.googlemail.com" in your SPF record:<br />
<blockquote>yourdomain.com.		300	IN	TXT	"v=spf1 a mx  include:aspmx.googlemail.com -all</blockquote><br />
<br />
Check your /var/log/mail.log to make sure it's working as expected:<br />
<blockquote>Aug 10 08:30:05 pizzapazzi postfix/smtpd[23061]: connect from mail-ob0-f199.google.com[209.85.214.199]<br />
Aug 10 08:30:12 pizzapazzi postfix/policy-spf[23074]: : SPF pass (Mechanism 'include:aspmx.googlemail.com' matched): Envelope-from: xxxxxxxxxx@yourdomain.com<br />
Aug 10 08:30:12 pizzapazzi postfix/policy-spf[23074]: handler sender_policy_framework: is decisive.<br />
</blockquote>
