---
layout: post
title: OpenWRT on Netgear WNDR3700, IPv6 edition!
categories: []
permalink: /archives/124-OpenWRT-on-Netgear-WNDR3700,-IPv6-edition!.html
s9y_link: http://www.lunesu.com/index.php?/archives/124-OpenWRT-on-Netgear-WNDR3700,-IPv6-edition!.html
date: 2011-06-08 22:46:21.000000000 +08:00
---
Happy IPv6 day everyone!<br />
<br />
<!-- s9ymdb:145 --><img class="serendipity_image_center" width="646" height="195"  src="http://www.lunesu.com/uploads/ipv6.PNG"  alt="" /><br />
<br />
Here's how I went about it:<br />
<ol><li>Triple-30 reset your router: with router on, press reset and hold; after 30 secs, pull the plug but keep reset; after 30 secs put back the plug, keep reset; after 30 secs release reset and power cycle.</li><br />
<li>Confirm triple-30 reset: router management page should ask for factory password. If not, retry triple-30 reset.</li><br />
<li>With router off, hold reset and turn on router. Keep reset until power led blinks green (After ~35 secs.)</li><br />
<li>tftp -i 192.168.1.1 PUT <a href="http://downloads.openwrt.org/backfire/10.03/ar71xx/openwrt-ar71xx-wndr3700-squashfs-factory.img">openwrt-ar71xx-wndr3700-squashfs-factory.img</a></li><br />
<li>Do nothing! Wait until the firmware is completely initialized. Check with ping -t 192.168.1.1</li><br />
<li>Access http://192.168.1.1/ (Luci) and change root password under Administration. There's no default password.</li><br />
<li>Still in Luci, configure your wan interface. Apply &amp; save. Reboot.</li><br />
<li>SSH to root@192.168.1.1</li><br />
<li>sysupgrade http://downloads.openwrt.org/snapshots/trunk/ar71xx/openwrt-ar71xx-generic-wndr3700-squashfs-sysupgrade.bin</li><br />
<li>opkg install kmod-ipv6 radvd ip kmod-ip6tables ip6tables 6to4 kmod-sit</li><br />
<li>vim /etc/config/network</li><br />
<li>Add to end:
```
config interface 6rd<br />
&#160;&#160;&#160;&#160;option proto 6to4<br />
&#160;&#160;&#160;&#160;option adv_subnet 1
```
</li><br />
<li>vim /etc/config/radvd</li><br />
<li>Change interface option 'ignore' to 0</li><br />
<li>/etc/init.d/network restart</li><br />
<li>In Luci, add the '6rd' interface to the wan zone</li><br />
<li>In Luci, add the following firewall rule: name 6to4, source wan, protocol 41, target accept. Save.</li><br />
<li>In Luci, make sure radvd starts at startup. Apply &amp; Save. Reboot.</li><br />
</ol>
