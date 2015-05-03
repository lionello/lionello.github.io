---
layout: post
title: Some PmWiki tricks
categories: []
permalink: /archives/79-Some-PmWiki-tricks.html
s9y_link: http://www.lunesu.com/index.php?/archives/79-Some-PmWiki-tricks.html
date: 2009-08-09 16:45:33.000000000 +08:00
---
I'm a fan of <a href="http://www.pmwiki.org/" title="PmWiki">PmWiki</a> because it's so easily installed and needs no database. It only requires PHP on the server. The wiki pages are kept as simple files, with makes it very easy to backup and restore. Skinning is also very easy: you provide a template.html file in which you use some special tags. These tags will be replaced with actual content when the page is requested.<br />
<br />
Apart from the skinning, there are two customizations that I did to my PmWiki installation.<br />
<br />
The first is using https for the password form. Although PmWiki supports https, it's an all-or-nothing approach: it will use the same scheme (http or https) all through the site. This puts unnecessary load on my server so I wanted to fix this. There are some hints on how to achieve this on the <a href="http://www.pmwiki.org/wiki/Cookbook/SwitchToSSLMode" title="PmWiki - Switch To SSL Mode">SwitchToSSLMode</a> page, but it seemed pretty complicated. In the end I tried one of the suggestions from the talk page, which basically uses https for the password form's POST and http for everything else. For this I had to change one line in pmwiki/scripts/forms.php, line 244. On that line the HTML for the form is constructed. I've added <em>{$SecureScriptUrl}</em> to start of the <em>action</em> attribute's value so I code override the host address for the form. In the <em>config.php</em> I then added a line to set the <em>$SecureScriptUrl</em> to <em>"https://pizzapazzi.com"</em>. I've also force the <em>$ScriptUrl</em> to http:// so that only the POST action uses https, everything else will refer back to http.<br />
<br />
The other customization had to do with cleaning up the URLs shown in the browser. By default a PmWiki URL will look like <em>http://example.com/pmwiki/pmwiki.php?n=Main.HomePage</em>. A couple of different approaches are described on the <a href="http://www.pmwiki.org/wiki/Cookbook/CleanUrls" title="PmWiki - CleanUrls">CleanUrls</a> page. I went with the alternative using Alias, since the Apache mod_alias module was already enabled by default. I created an alias /wiki to point to <em>/var/www/pmwiki/pmwiki.php</em>, all from within Webmin. To clean up the initial URL, I've added a file <em>/var/www/index.php</em> which does an server side include of <em>pmwiki/pmwiki.php</em>.<br />
<br />
Now to fix that skin.
