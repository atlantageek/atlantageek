--- 
layout: post
status: publish
published: true
title: Rocking ltree on postgresql
date: 2012-10-25 22:46:21 -04:00
author: atlantageek
author_login: atlantag
author_email: atlantageek@gmail.com
categories: 
- Uncategorized
tags: 
- postgres
- postgresql
- databases
comments: []

---

Postgresql 9.1 Now ships with a module called ltree. This is a cool postgresql that allows you to model hierarchies in you database and leverages the GIST indexing mechanism built into postgres.
Assume you are modeling several organizations:


<blockquote>CREATE TABLE orgs (path ltree);
INSERT INTO orgs VALUES ('Top');
INSERT INTO orgs VALUES ('Top.OrgA');
INSERT INTO orgs VALUES ('Top.OrgA.Division1');
INSERT INTO orgs VALUES ('Top.OrgA.Division1.storeZ');
INSERT INTO orgs VALUES ('Top.OrgA.Division1.storeY');
INSERT INTO orgs VALUES ('Top.OrgA.Division2');
INSERT INTO orgs VALUES ('Top.OrgA.Division2.storeV');
INSERT INTO orgs VALUES ('Top.OrgA.Division2.storeW')
CREATE INDEX path_gist_idx ON test USING gist(path);
CREATE INDEX path_idx ON test USING btree(path);
</blockquote>
Now you can do some interesting queries such as Give me all the rstores in Division1
<code>
select * from orgs where path ~ "Top.OrgA.Division1.*"
</code>
or give me all the stores (where the path is at least 4 items long)
<code>
select * from orgs where path ~ Top.{4}
</code>

This is another neat feature that Postgresql gives you but mysql doesn't
