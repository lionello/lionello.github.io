---
layout: post
title: How long is one month?
categories: []
permalink: /archives/111-How-long-is-one-month.html
s9y_link: http://www.lunesu.com/index.php?/archives/111-How-long-is-one-month.html
date: 2010-04-26 11:16:53.000000000 +08:00
---
Depends who you ask:<br />
<blockquote>mysql> select "2010-01-31" + INTERVAL 1 MONTH;<br />
+---------------------------------+<br />
| "2010-01-31" + INTERVAL 1 MONTH |<br />
+---------------------------------+<br />
| 2010-02-28                      |<br />
+---------------------------------+<br />
1 row in set (0.25 sec)</blockquote><br />
<blockquote>php > print date("Y-m-d",strtotime("2010-01-31 +1 month"));<br />
2010-03-03</blockquote><br />
<blockquote>sqlite> select date('2010-01-31',"+1 month");<br />
2010-03-03</blockquote>I'm siding with PHP/sqlite on this one.
