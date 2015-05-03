---
layout: post
title: Adding network storage to Windows 7 Libraries
categories: []
permalink: /archives/90-Adding-network-storage-to-Windows-7-Libraries.html
s9y_link: http://www.lunesu.com/index.php?/archives/90-Adding-network-storage-to-Windows-7-Libraries.html
date: 2009-11-08 17:39:41.000000000 +08:00
---
One of the things I like about Windows 7 are the <em>Libraries</em>. These are pseudo-folders that actually create a view of a collection of folders to make them appear as one. By default there are 4 libraries: Documents, Music, Pictures and Video. Each of the default libraries combines the user's folder with the respective public folder, so the <em>Music Library</em> contains the files from your private music folder "My Music" and the "Public Music" folder.<br />
<br />
Of course, it only seems logical that you'd want to add all remote folders to those libraries as well. This is allowed for external USB drives, but Windows 7 won't let you add network shares to the libraries! The help file suggests you mark the network share as "Make available offline" and then add the offline folder to the library, but this would basically copy all remote data to your local PC. That can't be right.<br />
<br />
There's a hack though. On NTFS you can make <em>softlinks </em>that point to files and folders, either local or remote. After creating a softlink to the remote folder (or share), the softlink can be added to the library.<br />
<br />
Example:<br />
```
C:\>mklink /d Remote \\Remote\share<br />
```
<br />
This will create a softlink in the C: root, pointing to the share "share" on the PC called "Remote". The /D creates a symbolic link to a folder. After creation, you can add the folder, or any subfolder, to the libraries.
