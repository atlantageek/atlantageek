--- 
layout: post
status: publish
published: true
title: Accessing a non standard database in rails
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
wordpress_id: 73
wordpress_url: http://www.atlantageek.com/?p=73
date: 2009-08-15 20:32:24 -04:00
categories: 
- Uncategorized
tags: 
- Ruby
- Mysql
comments: true

---
Normally in rails you would configure a production, test and development database in the database.yml file.  If some of your rails data comes from a different database you can configure it in database.yml as well.  For example I wanted two rails applications to synchronize their databases occassionally.
To do this I created a remote db in one of the rails application that looks like this in database.yml:

remotedb:
  adapter: mysql
  database: remote_rails
  username: remote_user
  password: remote_pwd
  host: remote server

Inside the script that is executed by script/runner I did the following to access the new db.

admin_cfg=Rails::Configuration.new.database_configuration\["remotedb"\]
if remote_cfg.nil?
  raise "admindb is not configured.  Cannot pull data from admin db."
end
dbh = Mysql.real_connect(remote_cfg\["host"\], remote_cfg\["username"\], remote_cfg\["password"], remote_cfg\["database"\])
Now use ruby's mysql library to work with dbh
