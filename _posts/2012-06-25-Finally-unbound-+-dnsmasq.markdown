---
layout: post
title: 'Finally: unbound + dnsmasq'
categories:
- Knowledge Base
permalink: /archives/128-Finally-unbound-+-dnsmasq.html
s9y_link: http://www.lunesu.com/index.php?/archives/128-Finally-unbound-+-dnsmasq.html
date: 2012-06-25 21:33:57.000000000 +08:00
---
I've been using <a href="http://unbound.net/" title="Unbound">unbound</a> as my only DNS server for over a year now. It's a recursive and caching DNS server with DNSSEC (a.o.) support.

This is particularly interesting for people living in China: the GFW's first line of defense (if you can call it that) is spoofing DNS. If you use the DNS server provided by any of the Chinese ISPs, you're unlikely to get any meaningful IP for domain such as facebook or twitter. But if you go through the whole recursive query yourself (ie. ask the root-servers for "com", ask the .com servers for "facebook" and ask the facebook servers for "www") then you will get an IP, which you can potentially verify using DNSSEC. Because facebook and others constantly add new servers to their datacenters, with new IPs, it's likely that once you get the right IP, you can actually connect to the server, without the need for VPN.

Unbound is easy to setup. For me, "opkg install unbound" was half the battle. The other half involved editing /etc/unbound/unbound.conf. For a detailed walk through, check <a href="https://calomel.org/unbound_dns.html" title="Calomel">this </a>great site [calomel.org]. Here's my current unbound.conf:
{% highlight sh %}
server:
        neg-cache-size: 0
        use-caps-for-id: yes
        prefetch: yes
        port: 533
        do-udp: yes
        do-tcp: yes
        interface: 0.0.0.0
        #interface: ::0
        #do-ip6: no
        val-clean-additional: yes
        harden-glue: yes
        harden-dnssec-stripped: yes
        rrset-cache-size: 1m
        rrset-cache-slabs: 2
        root-hints: "named.cache"
        auto-trust-anchor-file: "root.autokey"
{% endhighlight %}

All this was working fine, but the problem was that local names in my LAN no longer got resolved. Unbound doesn't no anything about DHCP and it won't accept DNS registrations from local machines. Dnsmasq does know about local names, since it's the one handing out the IPs in the first place. Ideally, I use both DNS servers: unbound for external queries and dnsmasq for internal (and internal only!) queries.

Most of the time I query external names, so I'd tried to get Unbound to forward local queries to Dnsmasq. I configured Unbound to use port 53 and Dnsmasq to use port 533. Then, I added a forward-zone to unbound.conf:
{% highlight sh %}
    forward-zone:
       name: "lan"
       forward-addr: 127.0.0.1@533
{% endhighlight %}
This was supposed to forward all queries in the "lan" domain (the local domain, as configured in dnsmasq/dhcp) to the dnsmasq server listening on port 533. Unfortunately, this didn't work. No queries got forwarded, even though a lookup for a local name did get suffixed with ".lan" by the clients.

So I flipped it around: dnsmasq on port 53, unbound on 533. Add the following to /etc/config/dhcp:
{% highlight sh %}
        option 'nonegcache' '1'
        list 'server' '127.0.0.1#533'
        option 'port' '53'
        option 'noresolv' '1'
{% endhighlight %}
This works: I can successfully ping my local computers and NAS, while still having other queries resolved (recursively) by unbound. I used <a href="https://calomel.org/dns_verify.html" title="DNS verification script">dns_verify.sh</a> to check my setup:
{% highlight sh %}
root@OpenWrt:~# ./dns_verify.sh

        ip        ->     hostname      -> ip
--------------------------------------------------------
ok      192.168.2.1 -> OpenWrt.lan. -> 192.168.2.1
ok      192.168.2.2 -> DaWang.lan. -> 192.168.2.2

DONE.
{% endhighlight %}
By the way, the script needs <strong>dig</strong>, which you can get by doing "opkg install bind-dig".
