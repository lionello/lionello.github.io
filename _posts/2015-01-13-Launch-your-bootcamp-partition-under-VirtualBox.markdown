---
layout: post
title: Launch your bootcamp partition under VirtualBox
categories: []
permalink: /archives/147-Launch-your-bootcamp-partition-under-VirtualBox.html
s9y_link: http://www.lunesu.com/index.php?/archives/147-Launch-your-bootcamp-partition-under-VirtualBox.html
date: 2015-01-13 15:55:04.000000000 +08:00
---
First, we add your current user account to the <strong>operator</strong> group and we give the anyone in the operator group read-write access to the raw partition. In my case it's <strong>/dev/disk0s4</strong>, but it might be different for you; mount the partition with <strong>Disk Utility</strong> and check the device with the <strong>mount</strong> command. We use <strong>VBoxManage</strong> to create vmdk wrapper for just partition <strong>4</strong> and make sure our regular user has access to the generated vmdk files. Add the top-level vmdk to your VM as a AHCI SATA drive; check SSD if applicable.<br />
```
sudo dseditgroup -o edit -a $(whoami) -t user operator<br />
sudo chmod 660 /dev/disk0s4<br />
sudo echo chmod 660 /dev/disk0s4 &gt; /etc/rc.local<br />
sudo VBoxManage internalcommands createrawvmdk -rawdisk /dev/disk0 -filename Win8.1-bootcamp.vmdk -partitions 4<br />
sudo chown $(whoami) Win8.1-bootcamp*.vmdk<br />
```
