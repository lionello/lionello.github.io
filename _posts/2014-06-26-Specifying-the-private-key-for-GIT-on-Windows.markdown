---
layout: post
title: Specifying the private key for GIT on Windows
categories: []
permalink: /archives/140-Specifying-the-private-key-for-GIT-on-Windows.html
s9y_link: http://www.lunesu.com/index.php?/archives/140-Specifying-the-private-key-for-GIT-on-Windows.html
date: 2014-06-26 12:27:46.000000000 +08:00
---
If GIT keeps asking for your password, even though you have private key auth set up, then its SSH probably doesn't know which key to use. Fix this by making a "config" file in your user's .ssh folder:<blockquote>echo IdentityFile ~/.ssh/github_rsa &gt; %USERPROFILE%\.ssh\config</blockquote>
