---
layout: post
title:  "Retooling for Market Success"
date:   2016-07-31 17:38:08 +0800
categories: update
author: Lionello Lunesu
---

This post is a condensed version of the talk I gave at [IoT Asia 2016](http://www.internetofthingsasia.com/) in Singapore, talking about the birth of an IoT device, its subsequent development, all the way up to (*SPOILERS* but not including) its crowdfunding.

<!--more-->

# May 2014
![](/images/Lionello Lunesu - Retooling for Success/02 May 2014 Ella concept.jpg)

This little device, nicknamed *ELLA*, was conceived at [Fail Faster Shanghai](https://www.flickr.com/photos/techyizu/albums/72157647007557576). Fail Faster is an event organized by Shanghai's tech enthusiasts at [Techyizu](http://techyizu.org/) (link down at time of writing) where in the span of one weekend, teams of random people come up with a startup concept and then try their best to validate and pivot (conform [The Lean Startup](http://theleanstartup.com)) in order to turn the initial idea into a viable company.

*ELLA* was such a startup concept: stop getting distracted by your phone and simply trust that *ELLA* provides you with up-to-date information, based on what's relevant to you in whichever context you find yourself. Near the door, *ELLA* would remind you to bring an umbrella in case of rain. At the office it would remind you of an upcoming meeting. *ELLA* does all this purposefully, without drawing too much attention to itself, simply by changing color and displaying an icon. It does not rely on your phone and in fact enables you to leave your phone in your pocket and not get distracted.

# November 2014
![](/images/Lionello Lunesu - Retooling for Success/03 Nov 2014 Web summit.jpg)

Fast forward 6 months. One of the members of the original *ELLA* team was considering exploring the initial concept. He and I joined forces and decided to take the upcoming [Web Summit](https://websummit.net/) as an opportunity for validation of the concept and possibly running into some angel investors.

![](/images/Lionello Lunesu - Retooling for Success/04 Nov 2014 Pre-summit hackathon team Honeycomb.jpg)

Before the actual Web Summit, [PCH](http://pchintl.com/) and [Dublin City University](http://dcu.ie) jointly organized a [Hardware Hackathon](https://blog.websummit.net/hardware-hackathon-3/) where we joined team *Honeycomb* to hack together a small IoT device for the office. The device had clear overlap with *ELLA*: both devices where connected and used light to communicate events, but Honeycomb was very limited in scope and basically was a Bluetooth enabled [Pomodoro timer](http://pomodorotechnique.com).

![](/images/Lionello Lunesu - Retooling for Success/05 Nov 2014 Honeycomb prototypes.jpg)

Thanks to the generosity of event sponsors [Nordic Semiconductors](http://nordicsemi.com/), the team created 3 working prototypes using the nRF51822 development dongle.

![](/images/Lionello Lunesu - Retooling for Success/06 Nov 2014 Cardboard case.jpg)

The 3D printer at the event was over scheduled, so we had to resort to other ways to hack a case.

![](/images/Lionello Lunesu - Retooling for Success/07 Nov 2014 Cardboard assembled.jpg)

The hackathon ended with each team pitching their idea to a jury consisting of investors, manufacturers, designers. *Honeycomb* won the design award, sponsored by [Design Partners](http://www.designpartners.com), resulting in Lio visiting their offices near Dublin and talking about potential cooperation.

![](/images/Lionello Lunesu - Retooling for Success/08 Nov 2014 Hackathon pitch.jpg)

The actual Web Summit event was eye-opening as well and we got tons of good feedback, from potential manufacturers and customers alike.

# December 2014
![](/images/Lionello Lunesu - Retooling for Success/09 Dec 2014 Hacked busybee.jpg)

Back in Shanghai, we got excited by all the attention we had gotten and decided to explore the Honeycomb angle for ELLA: an IoT device for the office. To kick off our user validation we had to create more prototypes and get them into the hands of customers. For the casing we opted for an off-the-shelves makeup jar. These initial prototypes got good feedback, but were so hacky that we soon spent more time fixing (and charging) them than actual user testing.

![](/images/Lionello Lunesu - Retooling for Success/10 Dec 2014 Taobao heaven.jpg)

When it comes to ordering parts and components, it's hard to imagine a place better suited than China. Order on [Taobao](http://taobao.com/) and within one day you'll get more stuff than you know what to do with.

# January 2015
![](/images/Lionello Lunesu - Retooling for Success/11 Jan 2015 Busybee PCBs.jpg)

Our first custom PCB, designed in [Cadsoft Eagle](https://cadsoft.io), featuring Atmel's low power, no-frills [ATtiny13A](http://www.atmel.com/devices/ATTINY13A.aspx), and Chinese made Li-Ion charger [TP4057](http://www.tp-asic.com/te_product_a/2009-05-20/33.chtml) and RGB LED [WS2812B](http://www.world-semi.com/en/Driver/Lighting/WS2811/WS212B/).

![](/images/Lionello Lunesu - Retooling for Success/12 Jan 2015 Busybee assembly.jpg)

A soldered and assembled device. The PCB is stuck to the LiPo battery with double sided tape.

![](/images/Lionello Lunesu - Retooling for Success/13 Jan 2015 Busybee usecase.jpg)

Repurposing a makeup case for IoT in the office. People liked the fusion of the analog and digital.

![](/images/Lionello Lunesu - Retooling for Success/14 Jan 2015 Busybee charging.jpg)

All devices charging using Qi-compatible wireless charging, ready to go to customers.

![](/images/Lionello Lunesu - Retooling for Success/15 Jan 2015 Busybee laser.jpg)

With the prototypes being round, as opposed to hexagonal, we needed a new name that would still convey the right message. We called these prototypes *Busybee*, subtly hinting at the original Honeycomb.

# February 2015
![](/images/Lionello Lunesu - Retooling for Success/16 Feb 2015 CNC Honeycomb.jpg)

We never stopped dreaming about ELLA. After getting more possitive feedback from our Busybee user testing, we came at a crossroads. Should we refocus our efforts on the simpler Busybee device, or see it as validation for the bigger vision? For better or worse, we chose the latter.

# March 2015
![](/images/Lionello Lunesu - Retooling for Success/17 Mar 2015 Prototypes.jpg)

By March we had created three more prototypes, experimenting with a tower form factor for Honeycomb. The biggest decision point was whether or not the device needed a high-resolution screen. This became an important design restriction.

![](/images/Lionello Lunesu - Retooling for Success/18 Mar 2015 Hex screen.jpg)

We created our own LED screen, fitting the hexagonal form factor we liked so much. This screen features 127 white LEDS, driven by the HT16K33 Holtek LED display controller. The PCB for this screen was generated by a [D](http://dlang.org/) program, which positioned the LEDs in an order that would minimize the number of required through-holes. While we liked the "hexscreen", our designer was not happy with how the LEDs were aligned. Most stock icons we're familiar with are aligned with the X and Y axes. Imagine, for example, what an email icon would look like on an hexagonal screen? The usual envelope icon looked terrible. Rendering text was limited to two characters.

# April 2015
![](/images/Lionello Lunesu - Retooling for Success/19 Apr 2015 OLED screens.jpg)

To be able to display any useful information, possibly text, we would have to bump the resolution, which made a DIY screen unattractive, for manufacturing and cost reasons. We started shopping for off-the-shelves OLED screens, both RGB and BW.

![](/images/Lionello Lunesu - Retooling for Success/20 Apr 2015 Cube.jpg)

Changing the form factor of the screen made us reconsider the shape of the device. Fitting a square screen in an hexagonal device wastes a lot of space, resulting in a large, uneven black border. A cube shaped device is the obvious choice.

![](/images/Lionello Lunesu - Retooling for Success/21 Apr 2015 Sandwich.jpg)

The internals of the device would get stacked up like a sandwich, from top till bottom: OLED screen, screen driver PCB, battery, main PCB.

![](/images/Lionello Lunesu - Retooling for Success/22 May 2015 Ella prototype.jpg)

The new cube prototype started to take shape. Here you can see the two PCBs. The top PCB features the Express-If ESP8266 Wi-Fi module. The bottom PCB features the nRF51822 BTLE module. We chose to implement both radios for two reasons: the Nordic bluetooth SoC consumed much less power than than the Wi-Fi SoC. We'd only power the Wi-Fi module to receive periodical updates. Secondly, having Bluetooth made the onboarding process more user friendly, without the burdensome switching of Wi-Fi networks which other IoT devices often required.

![](/images/Lionello Lunesu - Retooling for Success/23 Jun 2015 Ella aluminium.jpg)

The first assembled ELLA, in a CNC-ed aluminium case. The whole devices acted as a button as well.

# July 2015
![](/images/Lionello Lunesu - Retooling for Success/24 Jul 2015 Filming.jpg)

With one sexy, working prototype we were ready to start filming our crowdfunding campaign. (Many successful teams have started with much less.) Our good friends at [Mirage Makers](http://www.miragemakers.co/) helped us tremendously. In this scene, the team members talk about the prototyping process on location at [Xinchejian](http://xinchejian.com), China's first hackerspace, where all of our prototyping took place.

![](/images/Lionello Lunesu - Retooling for Success/25 Jul 2015 Filming at airbnb.jpg)

For the actual crowdfunding intro video we rented a nice apartment on AirBnb and got our friend Xinxin from [REBYXINZHAO](http://rebyxinzhao.com) to act in it.

![](/images/Lionello Lunesu - Retooling for Success/26 Jul 2015 Ella at home.jpg)

Covering all the different scenarios we had in mind in a single crowdfunding video proved difficult. We tried to show how ELLA could help you with non-intrusive reminders, and how it could be used to control other connected devices like smart lighting on radios.

![](/images/Lionello Lunesu - Retooling for Success/27 Aug 2015 Brinc.jpg)

We realized that we needed help with the final push into crowdfunding. We got accepted into [Brinc's](http://brinc.io) IoT accelerator program, where we worked with their amazing team on defining our message and pushing through presales.

![](/images/Lionello Lunesu - Retooling for Success/28 Aug 2015 Continue yes no.jpg)

Alas, after months of focusing on our marketing and signups, we were unable to reach the required number of pledges that would guarantee a successful campaign. At this point the options were limited: we could either wing it, put the campaign online and hope that "they will come", or go back to the drawing board once again.

After little over one year of intense research and development, the ELLA project got shelved. We've learned a lot, made many friends and business relationships that continue to help to this day.
