---
layout: post
title: Shared GIT repo over SSH
categories: []
permalink: /archives/141-Shared-GIT-repo-over-SSH.html
s9y_link: http://www.lunesu.com/index.php?/archives/141-Shared-GIT-repo-over-SSH.html
date: 2014-07-03 13:40:20.000000000 +08:00
---
GIT over SSH will create files with user:group from the user that's doing the push. This will prevent other users from changing files, in particular updating refs/heads/.

To share a GIT repo over SSH among several users, create the repo with --shared=group and put all users in the same <em>primary </em>group:
{% highlight sh %}
sudo addgroup git
sudo adduser user1 git
sudo adduser user2 git
git init --bare --shared=group
{% endhighlight %}

To fix sharing for an already existent setup, fixup the primary groups and file ownership:
{% highlight sh %}
sudo cat /etc/group
sudo usermod -g 1006 -G 1007 user1
sudo usermod -g 1006 -G 1008 user2
sudo chown -R root:git /var/repo.git
sudo git config core.sharedRepository group
{% endhighlight %}
where 1006 is the ID of the 'git' group and 1007 and 1008 refer to the user's respective groups.
