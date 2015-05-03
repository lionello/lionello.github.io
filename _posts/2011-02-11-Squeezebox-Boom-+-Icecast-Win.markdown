---
layout: post
title: Squeezebox Boom + Icecast = Win
categories: []
permalink: /archives/120-Squeezebox-Boom-+-Icecast-Win.html
s9y_link: http://www.lunesu.com/index.php?/archives/120-Squeezebox-Boom-+-Icecast-Win.html
date: 2011-02-11 07:03:38.000000000 +08:00
---
Been to Hong Kong for Chinese New Year and one of the gadgets I bought there was a <a href="http://www.logitech.com/en-us/speakers-audio/wireless-music-systems/devices/4707" title="Logitech Squeezebox Boom">Logitech Squeezebox Boom</a> (1880HK$). I had heard about the Squeezebox line before, but had never actually seen one. I have looked on taobao.com but there were only few sellers and (hence?) more expensive than abroad. (There's one seller that seems to have quite a supply and selling them at half the market price, but that's just too shady.)<br />
<br />
<!-- s9ymdb:143 --><img class="serendipity_image_right" width="110" height="102"  src="http://www.lunesu.com/uploads/180px-Boom_200.serendipityThumb.jpg"  alt="" />The Squeezebox Boom has Wifi, so I just had to hook up the power and it was set. It detected my wifi network, and I entered my password. First surprise: the Squeezebox <strong>had to </strong>connect to a Squeezebox server. You can install your own (Squeezebox Server aka Squeezecenter) or you can let it use Logitech's mysqueezebox.com. It appears the Squeezeboxes are very thin clients, with most of the logic (and plugins, add-ons, apps...) handled by the server. I didn't feel like installing a server (it kind-of defeats the purpose of the Squeezebox, if you ask me) so I went with mysqueezebox.com. The first time you connect you must create an account, but this can be done right from the Squeezebox. <br />
<br />
After the initial setup I was shown the main menu. Because I was using mysqueezebox.com (as opposed to my own server) I could only choose "Internet Radio". However, it was a very extensive list of well known radio stations, and in about 10 minutes I was listening to Studio Brussels! The quality was really good, and definitely better than the <a href="http://us.store.creative.com/Creative-D100-Wireless-Bluetooth-Speakers-Black/M/B003N9SR00.htm" title="Creative Labs D100">Creative D100</a> speakers I had bought 1 month earlier.<br />
<br />
But still no access to my own music collection. Second surprise: the Squeezebox does not understand UPnP or DLNA. I thought that was a major bummer. It would have allowed me to simply hook it up to my <a href="http://www.buffalotech.com/products/network-storage/home-and-small-office/linkstation-live-ls-chl/" title="Buffalo LinkStation Live">Buffalo LinkStation Live</a>'s Twinkymedia Server. It seemed I had to install a Squeezebox Server.<br />
<br />
Squeezebox Server was installable on my Linkstation using: <blockquote>ipkg install squeezecenter</blockquote>That went well (albeit it was a somewhat older version), but it was immediately apparent that the 64Mb RAM of the Linkstation were not enough for the hefty perl + mysql Squeezecenter. The NAS was constantly swapping and became unresponsive.<br />
<br />
What I basically wanted was simple: when I still had my original Xbox with <a href="http://xbmc.org/" title="XBox Media Center">XBMC</a>, I had made a startup script that immediately started Party Mode as soon as the XBox was turned on. Filling the room with music was a simple act of pressing a button. Most of the time I didn't care about which CD was playing. (Without Party Mode I would spend minutes figuring out what music I felt like listening and usually ending up with one of my favorites, never choosing 90% of the music I actually had.)<br />
<br />
I had heard about <a href="http://www.icecast.org/" title="Icecast.org">Icecast</a> before. It's a music (media, to be more exact) streamer, similar to shoutcast. It was easily installed on my NAS using <blockquote>ipkg install icecast</blockquote>. Icecast itself is just the streaming server, it doesn't actually read music itself, but needs a Icecast Source to provide it with the bits. That job is done by <a href="http://www.icecast.org/ices.php" title="Icecast Source">Ices</a>, also installable using<blockquote>ipkg install Ices0</blockquote>(Apparently nobody has built Ices2 for the ARM yet?) The input for Ices is a text file with the playlist. Because Ices0 (as opposed to Ices2) only understand MP3, I had to make a playlist with only my MP3s, skipping Flac and Ogg:<blockquote>find /mnt/disk1/share/music -name \*.mp3 &gt; playlist.txt</blockquote><br />
Icecast and Ices could now be started with:<blockquote>/opt/bin/icecast -c /opt/etc/icecast.xml -b<br />
/opt/bin/ices -c /opt/etc/ices.conf.dist -B</blockquote>I could now connect to my own radio station from Winamp! I then proceeded to add the URL to my Favorites on mysqueezebox.com but it wouldn't work. The Squeezebox cannot play streams on the LAN because the server (and in this case, mysqueezebox.com) must be able to access the stream, for some metadata I presume. The solution was easy: open a random port on the router and forward the traffic to the Icecast server: success!<br />
<br />
One caveat though: the LinkStation would be constantly streaming music, even when nobody was listening, causing non-stop churning of the harddisk. I wanted to started Ices when the first listener connected and to stop it once the last listener went away. That could be done by adding an authentication section to icecast.xml:<pre name="code" class="xml">&lt;authentication type="url"&gt;
  &lt;option name="listener_add" value="http://localhost:8081/icecast.php"/&gt;
  &lt;option name="listener_remove" value="http://localhost:8081/icecast.php"/&gt;
  &lt;option name="auth_header" value="icecast-auth-user: 1"/&gt;
&lt;/authentication&gt;</pre>With these settings, the Icecast server would contact the mentioned URLs when a listener (dis)connected. In the PHP file I could then start/stop the Ices server:<pre name="code" class="php">&lt;?
$action = $_POST['action'];
$mount = $_POST['mount'];

header("icecast-auth-user: 1");

function store($c)
{
  $file = fopen("/tmp/count", "c+");
  $count = fgets($file);
  if ($count !== false)
    rewind($file);
  else
    $count = 0;
  $count = intval($count) + intval($c);
  fputs($file, $count."\n");
  fclose($file); 
  return $count;
}
  
function start_ices()
{
  $array = array();
  exec("ps", $array);
  foreach($array as $line)
    if (strpos($line, "/opt/bin/ices") !== false)
      return;
  exec("/opt/bin/ices -c /opt/etc/ices.conf.dist -B");
  sleep(1);//echo $i;
}

function stop_ices()
{
  exec("kill `cat /opt/var/log/icecast/ices.pid`");
}

if ($action == "listener_add" &&amp; $mount == "strm" &&amp; store(1) == 1)
  start_ices();
if ($action == "listener_remove" &&amp; $mount == "strm" &&amp; store(-1) == 0)
  stop_ices();

?&gt;Success.
</pre>I can't believe how hard it is to create a global setting in PHP. I needed an atomic increment/decrement for the user count. The store($) function basically serves as a global InterlockedAdd, loading a integer from a tempfile, adding a value to it, writing the result back to the file and returning it to the caller.  Please let me know if there's a easier way to accomplice that.
