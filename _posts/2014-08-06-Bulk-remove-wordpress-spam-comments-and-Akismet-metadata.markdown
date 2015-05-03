---
layout: post
title: Bulk remove wordpress spam comments and Akismet metadata
categories: []
permalink: /archives/142-Bulk-remove-wordpress-spam-comments-and-Akismet-metadata.html
s9y_link: http://www.lunesu.com/index.php?/archives/142-Bulk-remove-wordpress-spam-comments-and-Akismet-metadata.html
date: 2014-08-06 17:09:54.000000000 +08:00
---
Connect to your wordpress DB using MySql workbench and execute this query:
{% highlight sh %}
DELETE FROM wp_comments WHERE comment_approved = 0;
DELETE FROM wp_commentmeta WHERE comment_id NOT IN (SELECT comment_id FROM wp_comments);
DELETE FROM wp_commentmeta WHERE meta_key LIKE '%akismet%';
{% endhighlight %}
