---
layout: post
title: "Nodejs-and-LCDproc"
categories:
- programming
tags:
- node.js
- raspberry pi
comments: true
---
![LCD display of bitcoin price](/img/posts/IMG_20131230_003453.jpg)

I developed my first node.js module today.  Its called lcdproc-client and provides a client interface for lcdproc. For those not in the know, lcdproc is a tcp server that controls most lcd displays (not the monitors but the blocky text ones you see on media players.) 

I wish these were more popular, In my opinion they should be on every desktop PC. On mine I wanted to display bitcoin prices  and I wanted to do it in either ruby or node.js. Ruby's library didnt work and I couldnt find one for node.js.  I did find one that worked in Perl but I didnt want to write perl code. Plus I had an idea that I might use this for a later raspberry pi project and node.js works well on node.js. I did use the perl code to capture the protocol to figure out how it worked.

Anyway the [lcdproc-client](https://npmjs.org/package/lcdproc-client) has been released so others can use lcdproc with node.js.  Also if you don't have a lcd screen you can use the curses simulation by installing lcdproc and running the daemon with curses LCD simulation (LCDd -d lis). 

Below is the code for the BTC display on the LCD.

	var Client = require('node-rest-client').Client;
	var LcdClient = require('lcdproc-client').LcdClient;
	
	lc = new LcdClient(13666,'localhost');
	
	function get_bitcoin()
	{
	  console.log("Get Bitcoin");
	  client = new Client();
	  client.registerMethod("jsonMethod", "http://blockchain.info/ticker", "GET");
	  client.methods.jsonMethod(function(data, response){
	            // parsed response body as js object
	    var obj = JSON.parse(data);
	            buy = obj.USD['buy'];
	            sell = obj.USD['sell'];
	        lc.widget_val("first_line",1,1,"BTC " );
	        lc.widget_val("second_line",1,2,"B:" + buy + " S:" + sell);
	        });
	}
	
	
	lc.on('ready', function() {
	  console.log("AAA");
	  console.log("WIDTH: " + lc.width);
	  console.log("HEIGHT: " + lc.height);
	  lc.screen("bacon");
	  lc.widget("first_line");
	  lc.widget_val("first_line",1,1,"This is a line");
	  lc.widget("second_line");
	  lc.widget_val("second_line",1,2,"This is a second line");
	  get_bitcoin();
	  setInterval(get_bitcoin, 300000);
	});
	lc.init();
