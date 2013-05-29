---
layout: post
title: "Raspberry PI and metageek Wi Spy web interface"
description: "Building A Web Interface for the Wi-Spy and using Raspberry PI as the web server."
category: 
tags: [raspberrypi wispy metageek websockets]
---
{% include JB/setup %}

##So what is it.
Its a web interface to a metageek spectrum analyzer that runs on a wi-spy. I have a [short video](http://www.youtube.com/watch?v=jj9u6VtkM3Y) demostrating an early version of the software.  
<iframe width="420" height="315" src="http://www.youtube.com/embed/jj9u6VtkM3Y" frameborder="0" allowfullscreen></iframe>

Currently there are markers and the beginning of a heatmap.

##What's the problem.
You may want to monitor your wi-fi signal over long periods of time without tying up your laptop.  With wi-spy's costing upware of $500 and raspberry PI costing in the $40 range it seems like a small investment to free your laptop.

Also I dont like installing specialized software on our laptops anymore.  To be it just hogs the system unnecessarily. I tend to prefer webapps over PC apps because they work well whether Im on my work map, home Linux machine or Tablet. 

But by connecting the Raspberry PI to the metageek wispy and with a little node.js code I now have a spectrum analyzer appliance I can connect to whenever I want to see the status of the spectrum.  This device could even be mounted in an area to monitor signal over time for a site survey.

##So how is it done.
The code can be downloaded from [github here](https://github.com/atlantageek/websocketsa)

After downloading you need to build the version of spectools that is in the repository.  The executable you need is call spectools_red.
* ./configure
* make spectool_red

Now run spectool_red as root.  This application copies data from wispy and copies it to the in memory redis database.

Next switch to the spectrum_analyzer screen and startup app.js with:
node app.js

This will start the web server.  If all is working well you will need to point the web browser to the raspberryPI's address port 8000.

The latest version has markers and a heat map.  

