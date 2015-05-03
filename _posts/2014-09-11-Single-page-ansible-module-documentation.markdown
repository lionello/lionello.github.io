---
layout: post
title: Single-page ansible module documentation
categories:
- Knowledge Base
permalink: /archives/143-Single-page-ansible-module-documentation.html
s9y_link: http://www.lunesu.com/index.php?/archives/143-Single-page-ansible-module-documentation.html
date: 2014-09-11 18:48:56.000000000 +08:00
---
A friend introduced me to <a href="http://www.ansible.com/home" title="Ansible">Ansible</a> recently and since then I've been spending a lot of time on writing playbooks. (Time which was perhaps better spent on <a href="http://docker.com" title="Docker">Docker</a>, but for now Ansible is my #shinynewthing.)<br />
<br />
Unfortunately I find the Ansible online docs lacking, especially given the amount of info available in `ansible-doc` command line tool.<br />
<br />
So, for all those Ansible lovers, here's a single page file with all of Ansible's modules and attributes:<br />
<a href="http://lunesu.com/ansible.yml" title="Ansible">http://lunesu.com/ansible.yml</a><br />
<br />
This file was created as such (with a little post-processing*):<br />
<blockquote>$ ansible-doc -l | cut -d ' ' -f 1 | xargs -n1 ansible-doc -s &gt;&gt;ansible.yml</blockquote><br />
<br />
* :%s/^\([^'#]\+\)'\([^#]*\)/\1`\2/g
