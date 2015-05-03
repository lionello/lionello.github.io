---
layout: post
title: How long is one month?
categories: []
permalink: /archives/111-How-long-is-one-month.html
s9y_link: http://www.lunesu.com/index.php?/archives/111-How-long-is-one-month.html
date: 2010-04-26 11:16:53.000000000 +08:00
---
Depends who you ask:
{% highlight sh %}
mysql> select "2010-01-31" + INTERVAL 1 MONTH;
+---------------------------------+
| "2010-01-31" + INTERVAL 1 MONTH |
+---------------------------------+
| 2010-02-28                      |
+---------------------------------+
1 row in set (0.25 sec)
{% endhighlight %}
{% highlight sh %}
php > print date("Y-m-d",strtotime("2010-01-31 +1 month"));
2010-03-03
{% endhighlight %}
{% highlight sh %}
sqlite> select date('2010-01-31',"+1 month");
2010-03-03
{% endhighlight %}
I'm siding with PHP/sqlite on this one.
