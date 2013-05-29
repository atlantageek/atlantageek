--- 
layout: post
status: publish
published: true
title: Verify ItemEditors input in a datagrid.
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 58
wordpress_url: http://www.atlantageek.com/?p=58
date: 2009-03-27 10:34:07 -04:00
categories: 
- flex
tags: []

comments: []

---
Working a lot with ItemEditors in DataGrids.

One of the issues I've been dealing with is adding validation to the item editor text entry. I've been attaching my validations to the item editor objects themselves however&nbsp; the Datagrid object throws out ItemEditEnd events. You have to check the event's DataGridReason to see whether it is a cancel or not and then you have the opportunity to validate the data.&nbsp; If the data is bad then do a event.PreventDefault to interrupt the data acceptance and set the target's errorString to the error message.

More details can be found here.

http://www.adobe.com/devnet/flex/articles/itemeditors_pt2.html
