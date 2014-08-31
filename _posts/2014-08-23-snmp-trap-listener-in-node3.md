---
layout: post
title: "snmp-trap-listener-in-node-part3"
categories:
- programming
- snmp
tags:
- node.js
comments: true
---

This article assumes you've read

* [Intro to node.js for snmp traps](/2014/08/23/snmp-trap-listener-in-node)
* [Setting up Linux and building your own traps](/2014/08/23/snmp-trap-listener-in-node2)

Setup your direcotry where you plan to do development. To build this application you will need to do the follo
wing assuming you have already installed node and npm on your system.

        npm install snmp
        npm install http
        npm install express
        npm install util
        npm install bunyan

The minimum code you need to write to see the snmp traps is the following.

    var os = require('os');
    var snmp = require('snmpjs');
    var http = require('http');
    var util = require('util');

    var trapd = snmp.createTrapListener();
    
    trapd.on('trap', function(msg){
           result.push(msg);
       var now = new Date();
       console.log("Trap Received " + now);
       console.log(util.inspect(snmp.message.serializer(msg)['pdu'], false, null));
       console.log(result.length);
       });
        trapd.bind({family: 'udp4', port: 162});
        
You'll see that trapd.on 

Using the snmptrap command from the previous article

     snmptrap -v 1 -m +TRAP-TEST-MIB -c public localhost TRAP-TEST-MIB::demotraps localhost 6 17 '' SNMPv2-MIB::sysLocation.0 s "You were here"

The output from the node output will be 
    
    Trap Received Sat Aug 30 2014 23:42:42 GMT-0400 (EDT)
    { op: 'Trap(4)',
      enterprise: '1.3.6.1.4.1.2021.13.990',
      agent_addr: '127.0.0.1',
      generic_trap: 6,  
      specific_trap: 17,
      time_stamp: 18277502,
      varbinds: 
       [ { oid: '1.3.6.1.2.1.1.6.0',
       typename: 'OctetString',
       value: <Buffer 59 6f 75 20 77 65 72 65 20 68 65 72 65>,
       string_value: 'You were here' } ] }

The strength of using node.js comes in when you want to integrate a web server with this functionality. In other programming languages to integrate a web server with a snmptrap server you would need to create multiple threads or to use the unix select command to listen on multiple file handles.  Both methods are kludgy. With node.js you just configure both a trap listener and a web server with a callback method and it just works. Here is a simple web server using the express network.  

    var os = require('os');
    var snmp = require('snmpjs');
    var http = require('http');
    var express = require('express');
    var util = require('util');

    var app = express();

    var result=[];

    app.use(express.static('public'));
    app.get('/get_today_count', function(req, res) {
       console.log(result.length);
      res.send(result.length.toString());
    });

    var server = app.listen(3001, function() {
      console.log('Listening on port %d', server.address().port);
    });


    var trapd = snmp.createTrapListener();

    trapd.on('trap', function(msg){
       result.push(msg);
       var now = new Date();
       console.log("Trap Received " + now);
       console.log(util.inspect(snmp.message.serializer(msg)['pdu'], false, null));
       console.log(result.length);
    });

    trapd.bind({family: 'udp4', port: 162});

This code uses the express modlue. So any calls to static files will go to a public directory.  The initial index.html file will run javascript that every few seconds makes an ajax cll to /get_today_count.  To get this server along with the public/index.html pull the code from [github](https://github.com/atlantageek/node-and-snmp/tree/master/code).

The screen will look like this:
![screenshot](https://raw.githubusercontent.com/atlantageek/node-and-snmp/master/images/snmp_count.png)

Part 4 is coming
