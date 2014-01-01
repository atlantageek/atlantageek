--- 
layout: post
status: publish
published: true
title: Radio Buttons act more like Combobox
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 59
wordpress_url: http://www.atlantageek.com/?p=59
date: 2009-04-01 09:43:25 -04:00
categories: 
- flex
tags: []

comments: true

---
Normally what I would do with radio buttons is to look for a click event and set variables accordingly.&nbsp; However there is another way to use radio button if you are operating more in a 'filling out a form' approach.

Here it is
	<?xml version="1.0" encoding="utf-8"?>
	<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml" layout="absolute">
	<mx:Label text="{'Radio Selected :'+ rg.selectedValue.toString()}"/>
	<mx:RadioButtonGroup id="rg"/>
	<mx:RadioButton x="10" y="26" value="RADIO 1" selected="true" label="radio1" groupName="rg"/>
	<mx:RadioButton x="10" y="52" value="RADIO 2" label="radio2" groupName="rg"/>
	</mx:Application></pre>
It now works more like a combobox instead of individual buttons. The label will alwys show the button group's selected Value.
