---
layout: post
title: Writable NTFS on Yosemite
categories: []
permalink: /archives/148-Writable-NTFS-on-Yosemite.html
s9y_link: http://www.lunesu.com/index.php?/archives/148-Writable-NTFS-on-Yosemite.html
date: 2015-01-13 21:50:41.000000000 +08:00
---
Remove osxfuse if installed via homebrew:<blockquote>brew uninstall osxfuse</blockquote><br />
<br />
Install <a href="http://sourceforge.net/projects/osxfuse/files/latest/download?source=files" title="osxfuse">osxfuse</a> binary and choose to install the MacFUSE compatibility layer<br />
<br />
Reboot (optional but recommended by osxfuse)<br />
<br />
Install ntfs-3g via homebrew: <blockquote>brew update &&amp; brew install ntfs-3g</blockquote><br />
<br />
Link mount_ntfs:<blockquote>sudo mv /sbin/mount_ntfs /sbin/mount_ntfs.original<br />
sudo ln -s /usr/local/Cellar/ntfs-3g/2014.2.15/sbin/mount_ntfs /sbin/mount_ntfs</blockquote><br />
<br />
Reboot
