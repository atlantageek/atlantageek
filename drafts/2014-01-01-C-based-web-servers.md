---
layout: post
title: "C Based web servers."
categories:
- programming
tags:
- C
- Web

---

Happy New Year!

Though C is considered the lingua franca of the linux world it is not very popular in the world of web development. This is rather sad because there are times when you do need a fast small footprint single purpose language. A popular scenario is for the embedded world. No body wants to run rails on a raspberry pi. And though node.js does run on pi, there is not a library for every technology yet for node.js (Tokyo Cabinet is missing in this world) However writing a web server in C may be the perfect fit for any sort of data collecting embedded system.

Embedded systems that collect data also often require a real-time component to them. So websockets can be a great help to support an embedded system.

[libwebsockets](http://libwebsockets.org) is a great solution for building a web server that also supports web sockets in C/C++. In this article I will show how libwebsockets.org can be used. 
