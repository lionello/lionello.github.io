---
layout: post
title: Shared GIT repo over SSH
categories: []
permalink: /archives/141-Shared-GIT-repo-over-SSH.html
s9y_link: http://www.lunesu.com/index.php?/archives/141-Shared-GIT-repo-over-SSH.html
date: 2014-07-03 13:40:20.000000000 +08:00
---
GIT over SSH will create files with user:group from the user that's doing the push. This will prevent other users from changing files, in particular updating refs/heads/.<br />
<br />
To share a GIT repo over SSH among several users, create the repo with --shared=group and put all users in the same <em>primary </em>group:<blockquote>sudo addgroup git<br />
sudo adduser user1 git<br />
sudo adduser user2 git<br />
git init --bare --shared=group</blockquote><br />
<br />
To fix sharing for an already existent setup, fixup the primary groups and file ownership:<blockquote>sudo cat /etc/group<br />
sudo usermod -g 1006 -G 1007 user1<br />
sudo usermod -g 1006 -G 1008 user2<br />
sudo chown -R root:git /var/repo.git<br />
sudo git config core.sharedRepository group</blockquote><br />
where 1006 is the ID of the 'git' group and 1007 and 1008 refer to the user's respective groups.
