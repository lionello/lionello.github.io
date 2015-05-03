---
layout: post
title: Using custom compression profiles in Movie Maker
categories: []
permalink: /archives/76-Using-custom-compression-profiles-in-Movie-Maker.html
s9y_link: http://www.lunesu.com/index.php?/archives/76-Using-custom-compression-profiles-in-Movie-Maker.html
date: 2009-08-08 17:49:48.000000000 +08:00
---
The default compression choices in Microsoft's Movie Maker are rather slim, but fortunately it's possible to add your own compression profiles. To create custom profiles, first install the latest <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=5691ba02-e496-465a-bba9-b2f1182cdf24&displaylang=en" title="Windows Media Encoder 9 Series">Windows Media Encoder 9</a>. This also installs a tool called <strong>Windows Media Profile Editor</strong>. Use it to create or edit profiles. <br />
<br />
In order for your custom profiles to show up in Movie Maker, save them into <em>C:\Program Files\Movie Maker\Shared\Profiles</em>. Note that in Vista you'll require elevation to write in that folder. It's easier to edit that folder's security setting first, to allow a non-elevated user account to write/modify files there. After you restart Movie Maker, your new profiles will show up when you publish for "playback on this PC."
