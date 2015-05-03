---
layout: post
title: Converting mailbox format from mbox to Maildir
categories: []
permalink: /archives/112-Converting-mailbox-format-from-mbox-to-Maildir.html
s9y_link: http://www.lunesu.com/index.php?/archives/112-Converting-mailbox-format-from-mbox-to-Maildir.html
date: 2010-04-29 08:43:03.000000000 +08:00
---
The default mailbox format used by postfix and dovecot is the mbox format. This basically boils down to a single file per user in <em>/var/mail/</em> with all mails concatenated, one after the other. (Mail in folders other than inbox are stored in ~/mail/)<br />
<br />
Mail stored in the Maildir format uses 1 file per mail. Accessing individual mails is faster, but because there are many small files, the disk space overhead is bigger.<br />
<br />
The mbox format is fairly simple and compact, but has a couple of problems. For one, being a single flat file it's easy to append new incoming mails, but it's not straightforward to remove a mail from the middle of the file. Another problem that I've noticed in Thunderbird 3 is that sometimes two mails would appear to be combined. I'd suddenly see another mail's attachments appearing on a mail without attachments. I'm not sure whether this is a problem with Thunderbird, dovecot, or perhaps a corrupt mbox file?<br />
<br />
The steps:<br />
<ol><li/>Before we start the conversion exit your mail client. <br />
<br />
<li/>You should also stop postifx to prevent mail being delivered to the wrong mailbox during conversion: <em>sudo /etc/init.d/postfix stop</em><br />
<br />
<li/>The easiest way to convert the mailbox format is to let dovecot do it for us. Make the following changes to <em>/etc/dovecot/dovecot.conf</em>: <blockquote>mail_location = maildir:~/Maildir<br />
mail_plugins = convert<br />
convert_mail = mbox:~/mail:INBOX=/var/mail/%u</blockquote>(Search for the settings and change them.)<br />
<br />
<li/>Apply the changes by restarting dovecot: <em>sudo /etc/init.d/dovecot restart</em><br />
<br />
<li/>Now start your mail client. Dovecot will convert the mailbox at login, so this might take a while. Also, chances are the client will start to download all mail again. (There seem to be <a href="http://wiki.dovecot.org/Migration/MailFormat" title="Converting dovecot mail storage format">some scripts</a> that can prevent this.)<br />
<br />
<li/>Before we start postfix, edit <em>/etc/postfix/main.cf</em> to tell it where to deliver the mail: <em>home_mailbox = Maildir/</em><br />
<br />
<li/>Restart postfix: <em> sudo /etc/init.d/postfix start</em></ol>
