---
layout: post
title: 'grub-install: stage1 not read correctly'
categories: []
permalink: /archives/53-grub-install-stage1-not-read-correctly.html
s9y_link: http://www.lunesu.com/index.php?/archives/53-grub-install-stage1-not-read-correctly.html
date: 2007-10-05 09:21:00.000000000 +08:00
---
I'm typing this entry under Ubuntu 8.04, booted from an external USB disk. The initial installation of ubuntu was pretty straightforward, just make sure the external drive is connected when you boot the first time and in my case I had to replace all occurrences of <em>hd1</em> with <em>hd0</em> in  <em>/boot/grub/menu.lst</em> after installation.<br />
<br />
Afterwards I decided I wanted linux on a different external drive, so I copied the partition from one drive to the other using gparted copy-paste. Again, pretty straightforward, but remember to shrink the source partition first to prevent wasting time copying free space. Remember to install grub after copying: <em>sudo grub-install --recheck /dev/sdc1 --root-directory=/mnt/x</em><br />
<br />
All was well, until I noticed a new entry in grub's boot menu, <em>Dell Utility Partition</em>, and I tried to boot from it. Not only did it not boot, it prevented linux from booting ever again.<br />
<br />
To make a long story short, what happened was that something in the Dell Utility Partition marked the first partition as FAT16. Normally the first partition of the drive would be the Dell Utility Partition, but in this case I booted from another drive and the first partition was my linux partition.<br />
<br />
Booting from my original linux and reinstalling grub using the above command line failed with error <em>/mnt/x/boot/grub/stage1 not read correctly</em>. This error is printed by the <em>/usr/sbin/grub-install</em> script where it checks whether grub can access the /boot/grub/stage1 file. This command failed because the partition got marked as FAT16. Apparently, mount doesn't care about the partition's type and will actually detect the file system, but grub just checks the type and uses the appropriate file system handler.<br />
<br />
The fix: use fdisk to fix the partition type. Run <em>sudo fdisk /dev/sdc</em>, print the partition table using <em>p</em>, change the type of the partition using <em>t</em>. ext3 is partition type <em>83</em>. After this, grub-install should complete successfully.
