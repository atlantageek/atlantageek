---
layout: post
title: "DRAFT: nodejs-and-lcdproc"
---
![Alt text][LcdScreen]

I've developed my first node.js module today.  Its called lcdproc-client and provides a client interface for lcdproc. For those not in the know, lcdproc is a tcp server that controls most lcd displays (not the monitors but the blocky text ones you see on media players.) 
I wish these were more popular, In my opinion they should be on every desktop PC. On mine I wanted to display bitcoin prices (cause going to preev.com is too much effort) and I wanted to do it in either ruby or node.js. Ruby's library didnt work and I couldnt find one for node.js.  I did find one that worked in Perl but I didnt want to write perl code. Plus I had an idea that I might use this for a later raspberry pi project and node.js works well on node.js. I did use the perl code to capture the protocol to figure out how it worked.

Anyway the [lcdproc-client)[https://npmjs.org/package/lcdproc-client] has been released so others can use lcdproc with node.js.  Also if you don't have a lcd screen you can use the curses simulation by installing lcdproc and running the daemon with curses LCD simulation (LCDd -d lis). 

![LcdScreen]: img/posts/IMG_20131230_003453.jpg
