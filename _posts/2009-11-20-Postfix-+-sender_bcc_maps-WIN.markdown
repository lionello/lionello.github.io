---
layout: post
title: Postfix + sender_bcc_maps = WIN
categories: []
permalink: /archives/93-Postfix-+-sender_bcc_maps-WIN.html
s9y_link: http://www.lunesu.com/index.php?/archives/93-Postfix-+-sender_bcc_maps-WIN.html
date: 2009-11-20 16:43:51.000000000 +08:00
---
I've been using IMAP for my email for years now. I like it, because it allows me to access my mail from any device, anywhere on the globe. But what's been bothering for all those years is that, when using IMAP, you'll have to transfer your email <em>twice </em>if you want to keep a copy in the <strong>Sent </strong>folder. When you think of it, this makes perfect sense: first you send the email using SMTP. SMTP doesn't care about your "sent items" IMAP folder, so to put a copy of the sent message in that folder, you'll have to transfer it again using IMAP.

So far I have not been able to find a solution to this conundrum. But then somewhere I saw Postfix' <a href="http://www.postfix.org/postconf.5.html#sender_bcc_maps" title="sender_bcc_maps">sender_bcc_maps configuration parameter</a>. By using a sender-bcc-map, postfix can send a BCC to any user, depending on the sender of the message. Basically, if you use a simple 1:1 map like X->X then any message sent by user X will immediately be sent to user X's local mailbox, and all this happens server side so the client does not have to transfer the email twice!

To create a sender-bcc-map, first create a new file in the usual postfix hash format: one mapping per line, where each mapping consists of the sender address followed by destination address, separated by whitespace. As usual, after editing the map file, run <em><strong>postmap </strong>mapfile </em>to create the associated .db file.

Maybe you'll have noticed that using a sender-bcc-map the email being sent will end up in the sender's inbox. This is actually not a bad thing! Because the message ends up in the inbox, I can finally use Thunderbird's "threaded view"! My inbox looks a lot like a newsgroup now, and I must say I really enjoy it. It's certainly much easier to find a discussion when the original messages and the replies are interleaved. (Yes, like on gmail, stop it already.)

Whether you like it or not: this is definitely a case of killing two birds with one stone.
