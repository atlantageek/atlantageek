--- 
layout: post
status: publish
published: true
title: Tools to develop flex on linux
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 39
wordpress_url: http://www.atlantageek.com/?p=39
date: 2008-09-11 07:35:10 -04:00
categories: 
- flex
- linux
tags: []

comments: true

---
Sometimes you need to write a debug log when developing flex. The best way to do that in linux is first configure your ~/mm.cfg file with the following:

<code>ErrorReportingEnable=1
TraceOutputFileEnable=1</code>

Then either use the flash player debug plugin or use the standalone debug player.  I prefer the standalone because I can launch it from the command line right after a successful mxmlc compile. Both are in the tar file <a href="http://download.macromedia.com/pub/flashplayer/updaters/9/flash_player_9_linux_dev.tar.gz">here </a>

The log will by default be written at
~/.macromedia/Flash_Player/Logs/flashlog.txt
And no this is current as of 2008-9-11, well after the macromedia/adobe merge.  Adobe just hasnt bothered changing the path yet.
