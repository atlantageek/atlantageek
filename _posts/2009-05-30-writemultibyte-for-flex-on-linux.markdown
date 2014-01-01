--- 
layout: post
status: publish
published: true
title: writeMultibyte for Flex on Linux
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 60
wordpress_url: http://www.atlantageek.com/?p=60
date: 2009-05-30 00:06:21 -04:00
categories: 
- flex
- flash
- linux
tags: []

comments: true

---
writeMultiByte is not working for linux (though it is fine in Windows) in Flashplayer 10.&nbsp; It failed for me in the ByteArray class and I've seen other postings that the issue exist for the binarySocket class as well. At least for the us-ascii encoding.

A For loop around a writeByte is thework around
