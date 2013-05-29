--- 
layout: post
status: publish
published: true
title: SNMP and Ruby
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 21
wordpress_url: http://www.atlantageek.com/?p=21
date: 2008-02-27 13:19:20 -05:00
categories: 
- Ruby
tags: 
- Ruby
- snmp
- trap
- traps
comments: 
- id: 917
  author: jonathanjulian.com &raquo; Testing SNMP traps with snmptrapd
  author_email: ""
  author_url: http://jonathanjulian.com/2008/07/testing-snmp-traps-with-snmptrapd/
  date: 2009-03-03 13:54:13 -0500
  date_gmt: 2009-03-03 18:54:13 -0500
  content: "[...] thanks to atlantageek for one of the only blog posts out there about snmp and [...]"
---
Working with SNMP can be challenging. If you are a Newbie to SNMP and are trying to do something a little unusual (Writing a Ruby agent that sends snmp traps) you probably have trouble finding examples.  So I've been fighting SNMP for a few days and here are some notes.

Before you start writing ruby code make sure you can send and recieve snmp messages. In my case I was working with traps.  This is how I got it working in Ubuntu.
<ol>
	<li>Install net-snmp (This is the snmp library and its daemons.) also install net-snmp-utils which is a lot of the command line tool. On Ubuntu <code> apt-get install net-snmp ; apt-get install net-snmp-utils</code> should work.</li>
	<li>On Ubuntu the /etc/default/snmpd file overrides a lot of the default snmp settings.  On my version the SNMPDOPTS variable included '127.0.0.1' as the address list.  You can just remove this if you want to recieve snmp traps from other servers.  Also change 'TRAPDRUN' to 'yes'.  This will start snmptrapd daemon.</li>
	<li>Try the following command.<code>snmptrap -v 1 -c public localhost 1.3.6.1.4.1.10300.1.1.1.12 localhost 3 0 ''</code> This should work.  We are not using any mibs. Something should be written to your /var/log/syslog</li>
</ol>
If you still have trouble with snmp you can use tcpdump to make sure you are getting a udp message on port 162. I won't get into that.  Its just a sanity check.

Now for the ruby code.  To send a version 1 message from ruby you need the snmp gem.  So do your 'gem install snmp'

Some sample code to do the same call

<pre>

require 'snmp'
SNMP::Manager.open(:Host => "localhost",:Version => :SNMPv1) do |snmp|
x=snmp.trap_v1(
"1.3.6.1.4.1.10300.1.1.1.12",
'localhost',
:enterpriseSpecific, #Generic Trap Type
0,1234
)

end
</pre>

Notice the :Host key in the Manager object. This is the address of the manager that you want to send the snmp message to.  The localhost in the parameters is the ip or domain name of the agent that is sending the trap.

These are just some quick notes mostly for myself and for others who get stuck.  It is probably not enough but there are so few examples of this out there on the net it should be useful.
