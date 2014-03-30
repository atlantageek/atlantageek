---
layout: post
title: "nodejs-as-a-windows-service"
categories:
- programming
- windows
tags:
- node.js
comments: true
---

To install node.js as a windows service you need three items.
* NSSM http://nssm.cc/download
* node.js for windows . http://nodejs.org/download
* Your node.js application.

Unzip your node.js code in a directory.  Let's say its c:\helloworld\hi.js
Install both nssm and node.js for windows.  Let's assume that node.js is in c:\Users\atlantageek\node.exe

Run 'nssm install' from the directory you installed nssm. A window will pop up.  On my version it looks similar to the following. 
*The path will be the path to node.js
* Startup directory will be the path where the js code and modules are located.
* The options will be the actual js appliction file. For my example it will look like the following.

![nssm service installer][nssm]

This is the very basic that you need.  You can also define Standard In and Standard Out redirection in the I/O tab.

Once the service is created. You can start and stop it from services manager thats built into windows.

[nssm]:/img/posts/nssm.png
