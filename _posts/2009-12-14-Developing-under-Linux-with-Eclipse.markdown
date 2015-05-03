---
layout: post
title: Developing under Linux with Eclipse
categories: []
permalink: /archives/94-Developing-under-Linux-with-Eclipse.html
s9y_link: http://www.lunesu.com/index.php?/archives/94-Developing-under-Linux-with-Eclipse.html
date: 2009-12-14 00:03:00.000000000 +08:00
---
I've installed Ubuntu 9.10 on my laptop (next to Windows 7) and actually, it's a pretty usable platform. I don't think there are many things that I do under Windows 7 that I cannot do under Ubuntu.

My favorite app for Windows, by far, is Visual Studio. A good, open-source alternative to Visual Studio is <a href="http://www.eclipse.org/" title="Eclipse">Eclipse</a>. I've used Eclipse before under Windows, mostly for writing some hobby stuff in the <a href="http://www.digitalmars.com/d/" title="The D Programming Language">D programming language</a>, because the <a href="http://www.dsource.org/projects/descent/" title="Descent plug-in">Descent</a> plug-in turns Eclipse in an amazing IDE for programming in D.

To prevent messing with tgz files (I never know where to unpack those) I've used Synaptic to install Eclipse. Because of this, my Eclipse was pretty lean and did not have the libraries needed by either Descent or the Android SDK. This was further aggravated by the fact that a problem in the installation package forgot to add the usual Eclipse update URL to the update sites list. Normally a 3rd party plug-in will automatically fetch all needed libraries from the Eclipse servers, but without any URLs, the installations were failing. This was fixed by adding one update site manually: in the Help menu, select Install new software, click Add, enter the following URL: http://download.eclipse.org/releases/galileo

There seems to be another bug in Eclipse 3.5 Galileo: sometimes the Install New Software dialog remains empty, but it's not really empty: the checkboxes and the tree collapse/expand buttons are there and work if you click on them, you just can't see them.

As always with linux, I'll get it working eventually, but it just takes a lot of time fixing these quirks <img src="http://www.lunesu.com/templates/default/img/emoticons/sad.png" alt=":-(" style="display: inline; vertical-align: bottom;" class="emoticon" />
