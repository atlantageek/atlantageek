---
layout: post
title: "Casing Up the Raspberry Pi, Part I"
categories:
- programming
tags:
- appliance
comments: true

---

The Problem
============
Raspberry Pi is a great little computer but easily the biggest weakness of the board is its power and/or usb ports. Imagine getting your new raspbery pi, unboxing it, plugging it in with the usb charge you stole from your kid's kindle, plugin your hdmi, keyboard and mouse and ..... nothing. Turns out a keyboard and a mouse can overwhelm your PI, and forget about that wifi adapter or the usb stick with all your rap mp3s. Youre just out of luck.

Eventually you will purchase a powered usb hub so your $35 computer is now $50 + stolen power supply.  Eventually your setup may look something like this.

![Raspberry Pi Nest][rpi_nest]

Lovely isn't it??!

This gets annoying quickly. After searching for a case that had an embedded usb hub I was disappointed to come up with nothing. There are 30 or more different commercially available cases and not a single one has a built in hub.  Theres a [home built project](http://www.adafruit.com/blog/2012/10/19/raspberry-pi-case-with-an-integrated-usb-hub-piday-raspberrypi-raspberry_pi/) that looks cool. Or a year old [empty promise](http://www.raspberrypi.org/phpBB3/viewtopic.php?t=29667). So I decided to build my own case with the plan being that its as simple and easily reproducable as possible. 

So I've been trying to think of an easy way to build this case. The issue with the raspberry pi is that unlike PC motherboards all ther ports are distributed around the board. 

![Raspberry Pi Nest][around]

So the cases are built skin tight around the board.

![Raspberry Pi Nest][skintight]

I wanted to do something different.  Not only did I want a usb hub but I was also inspired by RaspyFi, an excellent project, and wanted to embed the pi in a speaker. So at work's white elephant party I was able to snag this speaker

![Speaker][speaker]

So Im not a wood worker. I don't have the patience for the detailed work it requires, I never had the coordination for it, I dont want to fill up my garage with thousands of dollars of tools and last of all I like having all my fingers.  However I do have a few basic things like a drill and bits.  

Looking at this project what I decided I want to do is to pull the ports off the raspberry pi and usb hub and put them on the back of the speaker.  So this would require some cabling which isnt a problem.  The other issue is I cant cut a square hole to save my life. After thinking about it the solution was obvious.

CE Tech and Leviton both do these media wall ports for in wall cabling

![hdmi port][hdmi]

They also sell these wall plates.

![wall plate][plate]

So you can see where Im going with this.

![So far so good][sofar]

Home Depot did not have the usb ports.  I've ordered a couple and when I get those I'll finish the case and post a cost breakdown.


[rpi_nest]:/img/posts/Raspberry-Pi-Nest.jpg
[around]:/img/posts/RaspiModelB.png
[skintight]:/img/posts/tightcase.jpg
[speaker]:/img/posts/speaker.jpg
[hdmi]:/img/posts/hdmi.jpg
[plate]:/img/posts/plate.jpg
[sofar]:/img/posts/sofar.jpg
