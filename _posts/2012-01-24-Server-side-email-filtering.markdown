---
layout: post
title: Server-side email filtering
categories:
- Knowledge Base
permalink: /archives/125-Server-side-email-filtering.html
s9y_link: http://www.lunesu.com/index.php?/archives/125-Server-side-email-filtering.html
date: 2012-01-24 16:58:04.000000000 +08:00
---
I hate etc/ and everything in it, but I realize it might not be etc/'s fault. The fault might lie with Google and Bing: most of the results I get are people asking the same question on some forum. These threads often end without any answer. Or worse, wrong information. About half the threads eventually end up with the members discussing a complete different issue. Anyway, I figured it out. The following example assumes you're <a href="http://lunesu.com/index.php?/archives/112-Converting-mailbox-format-from-mbox-to-Maildir.html" title="Converting mailbox format from mbox to Maildir">using the <strong>maildir</strong> format for your mailbox</a>.
<ol><li>Create a file called <strong>.procmailrc</strong> in your home folder and add the following contents:
{% highlight sh %}
DEFAULT=$HOME/Maildir/
MAILDIR=$HOME/Maildir

:0:
* ^Mailing-list: list staff@somedomain\.com
.SomeFolder/
{% endhighlight %}
<li>Tell postfix to use <strong>procmail </strong>for mail filtering/processing by adding/changing the following line to /etc/postfix/main.cf:
{% highlight sh %}
mailbox_command = procmail -a "$EXTENSION"</li>
{% endhighlight %}
</li>
<li>Restart postfix
{% highlight sh %}
sudo /etc/init.d/postfix reload
{% endhighlight %}
</li>
</ol>
As you can see, the rules in .procmailrc use regular expressions to find the right mail. Each rule starts with colon-zero-colon (see <strong>man procmailrc </strong>for more options), followed by a line with the regular expression. Here, <strong>^</strong> means beginning of the line. Note that dots must be escaped with a backslash. A wildcard would be written as <strong>.*</strong>
The next line describes what to do when the regular expression is matched. In this case we want procmail to deliver the mail to a folder called SomeFolder. (If you're using <strong>mbox </strong>instead of maildir this would be a filename rather than a folder name.)

Add as many rules as you want, separating them with an empty line.
