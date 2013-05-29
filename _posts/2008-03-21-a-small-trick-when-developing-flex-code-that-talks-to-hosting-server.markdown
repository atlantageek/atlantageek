--- 
layout: post
status: publish
published: true
title: A small trick when developing flex code that talks to hosting server
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 23
wordpress_url: http://www.atlantageek.com/?p=23
date: 2008-03-21 22:05:52 -04:00
categories: 
- flex
- flash
tags: []

comments: []

---
I often develop my flex code on a machine that is not running a web server. So I have to hardcode the IP of a working server when I need to pull data from the hosting server.

This can be a pain because before you checkin the code you have to remember to change the method of pulling the IP address.

So here is my solution
<pre>
   var domain:String=mx.utils.URLUtil.getServerName(url);
  var port_nbr:int=mx.utils.URLUtil.getPort(url);
 if (flash.system.Security.sandboxType == "localTrusted")
  {
    Alert.show("In Development mode:"+domain+","+port);
    webService.wsdl="http://10.0.0.60:3000/services/wsdl";
  }
  else
  {
     webService.wsdl="http://"+domain+":"+port+"/services/wsdl";
  }</pre>
Obviously this is for web services but this technique can be used for sockets as well.
