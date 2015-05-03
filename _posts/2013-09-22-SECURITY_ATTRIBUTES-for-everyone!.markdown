---
layout: post
title: '"SECURITY_ATTRIBUTES for everyone!"'
categories: []
permalink: /archives/136-SECURITY_ATTRIBUTES-for-everyone!.html
s9y_link: http://www.lunesu.com/index.php?/archives/136-SECURITY_ATTRIBUTES-for-everyone!.html
date: 2013-09-22 11:42:43.000000000 +08:00
---
I keep having to Google this, so I thought it'd just put it up here. This piece of code creates a <em>SECURITY_ATTRIBUTES</em> (and <em>SECURITY_DESCRIPTOR</em>) for the <strong>Everyone</strong> group. Handy for some quick-n-dirty hacking, but probably A Bad Idea for anything else.<br />
<br />
{% highlight c %}
SECURITY_DESCRIPTOR SD;
InitializeSecurityDescriptor(&SD, SECURITY_DESCRIPTOR_REVISION);
SetSecurityDescriptorDacl(&SD, TRUE,(PACL)NULL, FALSE);

SECURITY_ATTRIBUTES sa = {0};
sa.nLength = sizeof(sa);
sa.bInheritHandle = FALSE;
sa.lpSecurityDescriptor = &SD;
{% endhighlight %}
<br />
