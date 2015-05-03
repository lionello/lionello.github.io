---
layout: post
title: Installing ubuntu packages from a newer repository
categories: []
permalink: /archives/98-Installing-ubuntu-packages-from-a-newer-repository.html
s9y_link: http://www.lunesu.com/index.php?/archives/98-Installing-ubuntu-packages-from-a-newer-repository.html
date: 2010-01-22 10:54:07.000000000 +08:00
---
As I've explained <a href="http://lunesu.com/index.php?/archives/93-Postfix-+-sender_bcc_maps-WIN.html" title="Postfix + sender_bcc_maps = WIN">earlier</a> I have my incoming and sent mail in a single IMAP folder. To make this folder more readable I use a Thunderbird mail filter to add a "Sent" tag to the outgoing mail. This also causes the sent mail to be colored differently from the received mail.

Unfortunately, the tags don't stick. They <strong>should</strong> be saved into the mail by using the <em>X-Keywords </em>header, but for some reason they aren't.

I've read somewhere this is a known problem with Dovecot 1.0.10, which is the version that comes with my server's Ubuntu 8.04 LTS. The LTS stands for Long Term Support, but unfortunately this bug is not considered important enough to release a new package for. Normally there's a good chance that the <em>-backports</em> repositories contain these kind of updates, but in this case hardy-backports did not have a newer version of Dovecot.

So how to install a newer version of a package, in a way that doesn't mess your system up too much?

The nice guys at <a href="http://www.beijinglug.org/" title="Beijing LUG">Beijing Linux User Group</a> pointed out that I can download the .deb file from a <a href="http://packages.ubuntu.com/intrepid/i386/" title="Ubuntu Intrepid Repository">newer repository</a>, and then install it using <em>sudo dpkg -i</em>. This works fine, but dpkg, as opposed to apt, does not automatically install dependencies. Fortunately for me I only had to manually update SQLite (using the same way) and the new Dovecot (from Intrepid) was good to go! After installing a package, it is advised to run <em>sudo apt-get -f update</em> in order to resolve any pending dependency issues.

Another solution that was suggested to me was to add the Intrepid repositories to the <em>/etc/apt/sources.list</em> but this is pretty scary, since it can cause a whole bunch of unpredictable updates. Some of this can be managed using apt pinning, by specifying what repository can be used for what package, but still a lot more complex than the manual installation of 3 deb files.
