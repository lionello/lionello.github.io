---
layout: post
title: Converting my mobile phone into an IR remote control, part 3
categories: []
permalink: /archives/108-Converting-my-mobile-phone-into-an-IR-remote-control,-part-3.html
s9y_link: http://www.lunesu.com/index.php?/archives/108-Converting-my-mobile-phone-into-an-IR-remote-control,-part-3.html
date: 2010-04-15 18:05:26.000000000 +08:00
---
After confirming the signal and still failing to turn on my TV, I figured my Panasonic Viera TV was using a different protocol from the RC5 that I had initially implemented. I tried to decode the protocol by pointing my remote at the IR sensor attached to the O-scope, but I was unable to get a complete code on the scope:<br />
<br />
<!-- s9ymdb:77 --><img class="serendipity_image_center" width="332" height="91"  src="http://www.lunesu.com/uploads/oscope3.png"  alt="" /><br />
<br />
It's immediately apparent however that the Panasonic IR protocol uses space modulation, where the spaces between the pulses are used to differentiate a 1 from a 0. But since I was not able to get a picture of a complete code, I still didn't know what signal I should create to turn my TV on! Fortunately, the <a href="http://lirc.sourceforge.net/remotes/" title="LIRC remote control database">LIRC remote control database</a> had an entry for just <a href="http://lirc.sourceforge.net/remotes/panasonic/N2QAYB000239" title="N2QAYB000239 remote control protocol">my remote</a> and I could confirm that the bits from the picture above are indeed a match for Panasonic's standby button.<br />
<br />
Above: .....000001000000001011110010......<br />
From LIRC: Power = 0x0100BCBD = ....00<strong>000001000000001011110010</strong>111101<br />
<br />
Now to change my code to write the right signal. Be right back...<br />
<br />
<strong>UPDATE: </strong>OK, so I actually got it to work using the audio out from my PC! I had to create a 72Khz wav to make it work. For some reason a 36Khz wouldn't work, but simply doubling the frequency (and duplicating all values) did the trick. Unfortunately, neither the 36Khz nor the 72Khz wav worked when played on my android mobile phone. In fact, the music player failed to load the 72Khz wav completely. Checking the LEDs with a digital camera when playing any loud wav file I could not see the LEDs light up the way they did when attached to my PC's audio out. Not enough power? Does the phone do some smart "ear piece present" check? More for tomorrow...
