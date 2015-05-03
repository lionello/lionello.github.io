---
layout: post
title: OpenWRT on Netgear WNDR3700, IPv6 edition!
categories: []
permalink: /archives/124-OpenWRT-on-Netgear-WNDR3700,-IPv6-edition!.html
s9y_link: http://www.lunesu.com/index.php?/archives/124-OpenWRT-on-Netgear-WNDR3700,-IPv6-edition!.html
date: 2011-06-08 22:46:21.000000000 +08:00
---
Happy IPv6 day everyone!

<!-- s9ymdb:145 --><img class="serendipity_image_center" width="646" height="195"  src="http://www.lunesu.com/uploads/ipv6.PNG"  alt="" />

Here's how I went about it:
<ol><li>Triple-30 reset your router: with router on, press reset and hold; after 30 secs, pull the plug but keep reset; after 30 secs put back the plug, keep reset; after 30 secs release reset and power cycle.</li>
<li>Confirm triple-30 reset: router management page should ask for factory password. If not, retry triple-30 reset.</li>
<li>With router off, hold reset and turn on router. Keep reset until power led blinks green (After ~35 secs.)</li>
<li>tftp -i 192.168.1.1 PUT <a href="http://downloads.openwrt.org/backfire/10.03/ar71xx/openwrt-ar71xx-wndr3700-squashfs-factory.img">openwrt-ar71xx-wndr3700-squashfs-factory.img</a></li>
<li>Do nothing! Wait until the firmware is completely initialized. Check with ping -t 192.168.1.1</li>
<li>Access http://192.168.1.1/ (Luci) and change root password under Administration. There's no default password.</li>
<li>Still in Luci, configure your wan interface. Apply &amp; save. Reboot.</li>
<li>SSH to root@192.168.1.1</li>
<li>sysupgrade http://downloads.openwrt.org/snapshots/trunk/ar71xx/openwrt-ar71xx-generic-wndr3700-squashfs-sysupgrade.bin</li>
<li>opkg install kmod-ipv6 radvd ip kmod-ip6tables ip6tables 6to4 kmod-sit</li>
<li>vim /etc/config/network</li>
<li>Add to end:
{% highlight sh %}
config interface 6rd
&#160;&#160;&#160;&#160;option proto 6to4
&#160;&#160;&#160;&#160;option adv_subnet 1
{% endhighlight %}
</li>
<li>vim /etc/config/radvd</li>
<li>Change interface option 'ignore' to 0</li>
<li>/etc/init.d/network restart</li>
<li>In Luci, add the '6rd' interface to the wan zone</li>
<li>In Luci, add the following firewall rule: name 6to4, source wan, protocol 41, target accept. Save.</li>
<li>In Luci, make sure radvd starts at startup. Apply &amp; Save. Reboot.</li>
</ol>
