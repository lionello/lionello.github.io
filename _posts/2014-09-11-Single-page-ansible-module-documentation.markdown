---
layout: post
title: Single-page ansible module documentation
categories:
- Knowledge Base
permalink: /archives/143-Single-page-ansible-module-documentation.html
s9y_link: http://www.lunesu.com/index.php?/archives/143-Single-page-ansible-module-documentation.html
date: 2014-09-11 18:48:56.000000000 +08:00
---
*Updated for ansible 2.2.1.0*

A friend introduced me to <a href="http://www.ansible.com/home" title="Ansible">Ansible</a> recently and since then I've been spending a lot of time on writing playbooks. (Time which was perhaps better spent on <a href="http://docker.com" title="Docker">Docker</a>, but for now Ansible is my #shinynewthing.)\*

Unfortunately I find the Ansible online docs lacking, especially given the amount of info available in `ansible-doc` command line tool.

So, for all those Ansible lovers, here's a single page file with all of Ansible's modules and attributes:
<a href="http://www.lunesu.com/ansible.yml" title="Ansible">http://www.lunesu.com/ansible.yml</a>

This file was created as such (with a little post-processing\*\*):
{% highlight sh %}
$ ansible-doc -l | cut -d ' ' -f 1 | xargs ansible-doc -s >ansible.yml
{% endhighlight %}

\* In fact, I now use Ansible to create my Docker containers.

\*\* :%s/^\\([^'#]\\+\\)'\\([^#]\*\\)/\1`\2/g

