---
layout: post
title: "snmp-trap-listener-in-node-part2"
categories:
- programming
- linux
tags:
- node.js
comments: true
---


Get your system to build traps
=======

This article assumes you've read [the first article](/2014/08/23/snmp-trap-listener-in-node)

Im going to assume a linux environment here. 

The first step is to start generating traps in your test environment. I'm borrowing a lot of this on [How to create a test trap](http://technotes.twosmallcoins.com/?p=369)


First we need to install net-snmp-utils.  This will give us a few command line tools and make sure the corresponding libraries are available. Now you should be able to type in snmptrap command and get a usage listing.

STEP 1
---
To install you do:

    $ sudo yum install net-snmp-utils net-snmp-devel

STEP 2
---
I'm not sure what test traps you have installed on the server so the next step is to create a test trap. To create a test trap is to we need to generate a mib file.  On Fedora systems you will find these in the /usr/share/snmp/mibs directory. 

If you want to confirm the directory run net-snmp-config --snmpconfpath to see the path of where mib files are searched for.

Use your favorite editor and create a test trap as follows:


TRAP-TEST-MIB.txt

    TRAP-TEST-MIB DEFINITIONS ::= BEGIN
    IMPORTS ucdExperimental FROM UCD-SNMP-MIB;

    demotraps OBJECT IDENTIFIER ::= { ucdExperimental 990 }

    demo-trap TRAP-TYPE
    STATUS current
    ENTERPRISE demotraps
    VARIABLES { sysLocation }
    DESCRIPTION "This is just a demo"
    ::= 17
    END
    

STEP 3
---
To send this trap we do the following

snmptrap -v 1 -m +TRAP-TEST-MIB -c public localhost TRAP-TEST-MIB::demotraps localhost 6 17 '' SNMPv2-MIB::sysLocation.0 s "You were here"

Breaking down the line is as follows.
* 'snmptrap' - command to send a trap
* '-v 1' - We are sending a SNMP version 1 trap
* '-m +TRAP-TEST-MIB' Look in the config path and load the mib file 'TRAP-TEST-MIB'
* '-c public' specifies that the community string is 'public'. A community string is like a password.
* 'localhost' The server we are sending the trap too
* 'TRAP-TEST-MIB::demotraps' - Which trap in TRAP-TEST-MIB that is being sent.
* 'localhost' Where the trap is coming from
* '6' Trap type. Can be a number from 1-6 (0-coldstart, 1-warmstart, 2-linkdown, 3-linkup, 4-authenticate failure, 5-egp neighborlost, 6-enterprise specific
* 17 - Trap ID , you see it in the mib file above.

Complete. 
Next step is a server to see the trap.
