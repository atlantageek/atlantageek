---
layout: post
title: "Setup Script Login"
categories:
- programming
tags:
- appliance
comments: true

---
A recent trend is to build small headless computer systems based on linux. This has come from the ability to run linux on very small computers (in size and power) for very utilitarian tasks (routers, NAS, data collection nodes). However these use cases make it inconvient to connect a keyboard, mouse and monitor to the linux machine. Often its because the device may not have a video out. Or the device's use case  does not require a monitor/keyboard to be connected in its normal operations. So when maintenance and/or administrative processes are necessary you need to provide some sort of interface/UI. You can invest hours into developing a cool Web UI that a administrator uses once every 6 months but sometimes a menu driven terminal app could do the job. 

So what are the reasons a terminal app would be preferred over a Web UI
*  You have administrative duties that need to be kept away from general users. 
*  You dont want anyone accidently accessing the screen during the appliance's normal use.
*  You have tasks that need to be completed before device is no the network (using keyboard/monitor )
*  The tasks that are triggered may require a bit of time to complete (doing a backup.)
*  You want something easily extendable.
*  Your support team is getting tired of describing where the menu/dropdown/button is on the webpage 
*  Sometimes a simple question/answer text interface is best so the administrator can be lead through the configuration.

This article will describe how to setup a menu driven idiot proof terminal app.

The first thing you need is a setup script. Download this [script](/references/setup-wrapper.sh) as an example. When run from the command line the initial screen looks like this.

	Thu Jan 2 00:35:02 EST 2014

	Welcome to your Admin console, please select an action.
	Please select action:
	
 	1) Run Setup App
 	2) Update Application
 	3) Restart Server
 	4) Configure Server Time
 	5) Backup System
 	80) Change Password
 	90) exit
	ENTER YOUR SELECTION:

This is fine but you want this to be easy. You dont want the user having to go to a command line, find the script location and then run it.  A solution to this is for the script to be started as soon as the user logs into the system from the command line.  So first lets create a setup user and configure this script as his login shell.

	cp setup-shell.sh /usr/local/bin #Copy shell script to /usr/local/bin
	useradd setup -m -s /usr/local/bin/setup-shell.sh #create the setup user
        passwd setup   #Configure the password for the setup user.

Now when you login as the setup user you will be given a menu.  When you exit out of the script you will be thrown back to the login screen.  

The last thing to do is give the setup user some privileges so that it can 'restart a server' or 'configure the server time'. If you refer back to the script you'll see that each command that needs root access is pre-pended with a 'sudo' command. We can list these commands in the sudoers file so that the setup user can only run the necessary commands. 

Here is the sudoers file I've created to go with this setup_shell.sh script.

	Cmnd_Aliases SETUPCMDS = /usr/bin/setup, /sbin/ifdown, /sbin/ifup, /usr/bin/yum, /sbin/service,/sbin/chkconfig, /bin/date
	setup   ALL=NOPASSWORD SETUPCMDS

SETUPCMDS is a list of all the commands setup needs acces to.
the next line says that tue user setup can connect from any server (through telnet/ssh/putty) or the console and run any of the SETUPCMDS commands without typing in the password.
