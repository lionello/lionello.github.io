---
layout: post
title: Mail troubles
categories:
- Knowledge Base
permalink: /archives/83-Mail-troubles.html
s9y_link: http://www.lunesu.com/index.php?/archives/83-Mail-troubles.html
date: 2009-09-10 22:52:02.000000000 +08:00
---
Back from holiday, no job, but my lunesu.com mail stopped working so that should keep me busy a couple of weeks.

The mail (like this site) was still hosted at directnic, so when their mail server stopped working this morning I had enough motivation to move my lunesu.com mail to my new VPS.

Since <a href="http://www.postfix.org/" title="The Postfix mail server">postfix </a>was up and running from the beginning, there was little left to do. I just had to instruct postfix to accept mail from all my domains, not just the main one from the hostname. This could be done from within webmin: <em>Server, Postfix Mail Server, General Options, What domains to receive mail for</em>. Select the last option. I just appended lunesu.com to the comma separated domain list. Click <em>Save and apply</em>.

Next step: fix the certificate warning I get when trying to send mail through my own server. The fix: in Webmin's Postfix page, select <em>SMTP Authentication And Encryption</em>. Change the paths of the certificate and private key files to the <a href="http://lunesu.com/index.php?/archives/80-Creating-my-own-keys.html" title="Creating my own keys">ones created before</a>. Save.

I also use a lot of aliases. Basically, I create a new mail alias each time a vague site wants my email address. Since I have about 25 aliases, I use Webmin's <em>Edit Map Manually</em> option in its <em>Mail Aliases</em> page. This works fine, but it won't be applied automatically after save! I had to run the following command as root for the aliases to start working:
{% highlight sh %}
newaliases
{% endhighlight %}
Another thing I noticed is that Postifx apparently always recognizes addresses of the form <em>account+suffix@domain</em> and delivers them to <em>account</em>, without the suffix. This how I made most of my aliases in the first place, so all of those with a plus sign could be removed from the aliases map. (Of course, the part before the plus sign must still be an existing account name or alias.)

So, most of it seems to be working at the moment, but no anti-spam yet, which is really the next thing I need to figure out before I get drowned in spam messages.
