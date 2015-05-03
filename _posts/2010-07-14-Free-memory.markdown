---
layout: post
title: Free memory
categories: []
permalink: /archives/117-Free-memory.html
s9y_link: http://www.lunesu.com/index.php?/archives/117-Free-memory.html
date: 2010-07-14 21:34:30.000000000 +08:00
---
My VPS was running out of memory so I started googling to see what could be done about it. The best tip was <a href="http://forums.spry.com/cpanel-whm/1420-high-memory-usage-look-into-mysqld.html" title="High Memory Usage">this</a>:
{% highlight sh %}
SSH into the system with putty or some other ssh utility
vi /etc/my.cnf
at the end of the section that starts with [mysqld] you want to add the following lines to turn off support for innodb and bdb.

skip-innodb
skip-bdb

restart mysql with '/etc/init.d/mysql restart'
{% endhighlight %}
My my.cnf happened to be in /etc/mysql/my.cnf, but after restart mysqld was using 80MB less memory! Before you do this though, make sure you don't have any tables using innodb or bdb.
