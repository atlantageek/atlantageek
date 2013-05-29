--- 
layout: post
status: publish
published: true
title: FlashPlayerTrust on Gnu/Linux
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 32
wordpress_url: http://www.atlantageek.com/?p=32
date: 2008-08-27 07:26:50 -04:00
categories: 
- flex
- flash
- development
tags: []

comments: []

---
I was getting Security Sandbox violations in my flash log. I was trying to read a local file and to connect to the network. This is for development reasons only. The final version is all web. Anyway to resolve this on my linux system I created a file with the top level paths that ended in .cfg. You can place  the file in the dir: /home/{user}/.macromedia/Flash_Player/#Security/FlashPlayerTrust directory.
For this to affect all users you put it in this directory.
/etc/adobe/FlashPlayerTrust/

The file could be as simple as

<code>
/home/{user}</code>

<code>

/tmp</code>

<code>&nbsp; </code>

This file should contain a list of top level paths you want the flash player to be able to find. As long as the file ends in .cfg you should be able to use any file name.
