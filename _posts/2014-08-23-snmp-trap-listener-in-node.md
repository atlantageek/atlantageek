---
layout: post
title: "snmp-trap-listener-in-node"
categories:
- programming
- windows
tags:
- node.js
comments: true
---
Developing a Trap Listener in Node.js
=========

A challenge in developing network monitoring applications is the need to listen to multiple inputs. A modern trap listener with a web interface must service both traps coming from the network as well as web requests coming from users or other service. There were acouple of ways to handle this but they always came with tradeoffs.

Threads
-------
Threads are one solution. One thread could listen on the snmp trap port and another listenr could monitor on the http port. However since these threads were often seperate processes it was difficult to share data between them.

IO.Select
---------
Another option was using IO.Select (or just select in c) The problem with IO select is that its clunky. Look at the following segment.




    r, w = IO.select(http_stream, snmp_stream)
    r.each do |stream|
      stream.handle_read
    end
    

You'll see in the example we have a list of streams to listen to and this is a blocking call.  You cant do much of anything else since select is a blocking call.  

Event Programming with Node.js
------------------------------

However node.js greatly simplifies this. For node.js you configure a listener and attach a callback to handle the event. The developer does not need to loop around a polling method or use a blocking method.  He just tells the program to listen for events and instead of waiting for the event he gives a function to call when that event occurs.

An example of this is below.


    var os = require('os');
    var snmp = require('snmpjs');
    var http = require('http');
    var express = require('express');
    var app = express();

    var result=[];

    //Setup http server code
    app.use(express.static('public'));
    app.get('/get_today_count', function(req, res) {
        console.log(result.length);
        res.send(result.length.toString());
    });

    var httpd = app.listen(3001, function() {
        console.log('Listening on port %d', server.address().port);
    });


    var trapd = snmp.createTrapListener();

    trapd.on('trap', function(msg){
        result.push(msg);
        var now = new Date();
        console.log("Trap Received " + now);
        console.log(result.length);
    });

    trapd.bind({family: 'udp4', port: 162});
    

In this code you can see that thw listeners have been configured.  A snmp trap listener and a httpd listener. Each listener has an inline function defined that will run whenever new data is available for this process.  In the following series of posts I will cover: Generating traps, building a trap listener.js, and using Dashboard-js to build a always on dashboard to monitor traps.

* [Setting up Linux and building your own traps](/2014/08/23/snmp-trap-listener-in-node2)
* [Building a Node.js Listener](/2014/08/23/snmp-trap-listener-in-node3)
* Building a Dashboard for your traps - coming soon.


