--- 
layout: post
status: publish
published: true
title: "adjusting innodb_log_file_size in  mysql to get around "
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
excerpt: " was getting \"ERROR 1033 (HY000): Incorrect information in file: './realworx_production/analyzers.frm'\" whenever I tried to adjust my mysql innodb parameters and run a query.\r\n\
  To adjust this you have do delete or move the ib_logfile0 and ib_logfile1 file in your datadir.  Then start the database."
wordpress_id: 77
wordpress_url: http://www.atlantageek.com/?p=77
date: 2010-05-11 16:57:22 -04:00
categories: 
- Uncategorized
tags: []

comments: true

---
I was getting "ERROR 1033 (HY000): Incorrect information in file: './realworx_production/analyzers.frm'" whenever I tried to adjust my mysql innodb parameters and run a query.
To adjust this you have do delete or move the ib_logfile0 and ib_logfile1 file in your datadir.  Then start the database.
So step-by-step
1. stop mysql
2. modify my.cnf, specifically the changes you want to make to innodb_log_file_size
3. move or delete ib_logfile1 and ib_logfile0
4. start database.
