---
layout: post
status: publish
published: true
title: Redesign of Wi-spy Web interface.
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
date: 2013-08-17 22:46:21 -04:00
categories: 
- Uncategorized
tags: 
- postgres
- postgresql
- databases
comments: []

---

So originally I wanted to build a web inteface for the wi-spy spectrum analyzer. I proved the concept with the first version.  You can watch the  [short video](http://www.youtube.com/watch?v=jj9u6VtkM3Y)
yourself that goes over the design. The biggest problem with the application was that it required a lot of infrastructure.  It required a special verion of spec_Tool to collect the data, node.js to supply the web server and a redis daemon to act inbetween.  When I started adding code to store off historical data I realized I'd need another technology such as tokyo cabinet and the like. This proved even more difficult because I need a locking mechanism between the reader and the writer.

Finally I decided that it was time to get to the basics.  The latest version of this tool is written totally in C (spectool_red) htat will collect data, supply the web interface and store the historical data in tokyo cabinet.  This is really only one executable.  More details can be found [here](https://github.com/atlantageek/websocketsa)
