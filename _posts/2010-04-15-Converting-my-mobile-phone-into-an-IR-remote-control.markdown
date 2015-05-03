---
layout: post
title: Converting my mobile phone into an IR remote control
categories: []
permalink: /archives/106-Converting-my-mobile-phone-into-an-IR-remote-control.html
s9y_link: http://www.lunesu.com/index.php?/archives/106-Converting-my-mobile-phone-into-an-IR-remote-control.html
date: 2010-04-15 11:39:36.000000000 +08:00
---
I had this silly idea a few weeks back: to simply attach an LED to an 3,5" audio jack and make a mobile phone application with which you can control all your devices, simply by playing a generated sound file.

So when I was in Shenzhen the other day I decided to buy an audio jack and some IR LEDs to keep myself busy in the hotel room. I attached a LED to the audio jack and played some mp3 on full volume. By using my phone's digital camera to look at the LED I could see it clearly light up, so at least the voltage of the audio out was high enough to drive a LED.

First problem I've encountered: the RC5 signal (and many other IR protocols) is modulated at 36Khz and there's no way my PC, let alone my mobile phone, will play a 36Khz tone. I slept on this for a few nights and suddenly had an idea: I use two LEDs! One will shine on a positive signal and the other on a negative signal. This way I'll be able to get the LEDs to flash up to a <em>theoretic </em>frequency of 44.1Khz. In practice this limit is lower because audio-out has a high pass (and low pass) filter, but with some luck I'll be able to get a decent 18Khz AC signal to achieve the required 36Khz modulation.

I quickly wrote a program (in <a href="http://digitalmars.com/d/" title="D Programming Language">D</a>, of course) to generate a .wav file with a modulated RC5 signal to turn TVs on or off. The RC5 signal consists of 14 data bits, each data bit is sent as two modulated bits: a data bit 1 is sent as 01 (a pause followed by the modulated signal) and a data bit 0 is sent as 10 (signal followed by a pause.)

Unfortunately, when I played this generated .wav file and held my laptop in front of my TV, nothing happened. In order to debug this, I bought a cheap O-scope on <a href="http://taobao.com/" title="Taobao online store">taobao</a>: only 350 RMB! Below you'll see the IR signal with the 14 bits:

<!-- s9ymdb:75 --><img class="serendipity_image_center" width="581" height="148"  src="http://www.lunesu.com/uploads/oscope.png"  alt="RC5 signal" />

The signal appears to be correct, so there must be something else wrong. Are the modulated pulses too wide, not wide enough? Is my TV <strong>not </strong>using the RC5 protocol?

To be continued...
