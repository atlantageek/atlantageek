---
layout: post
title: "Useful Queries to admin postgres"
categories:
- programming
tags:
- postgres
- postgresql
- databases

---

Here are some of my favorite postgres queries to help administrate postgres.


Identify slowest running queries.
---------------------------------

	SELECT
  		pid,
 		current_timestamp - xact_start as xact_runtime,
 		query
	FROM pg_stat_activity
	ORDER BY xact_start;

Identify existing locks in postgres, good for finding deadlocks.
---------------------------------------------------------------

	SELECT pg_class.relname,pg_locks.* 
	FROM pg_class,pg_locks 
	WHERE pg_class.relfilenode=pg_locks.relation;


See foreign key constraints.
----------------------------

	SELECT c.constraint_name
    		, x.table_schema as schema_name
    		, x.table_name
    		, x.column_name
    		, y.table_schema as foreign_schema_name
    		, y.table_name as foreign_table_name
    		, y.column_name as foreign_column_name
	FROM information_schema.referential_constraints c
		join information_schema.key_column_usage x
    		on x.constraint_name = c.constraint_name
		join information_schema.key_column_usage y
    		on y.ordinal_position = x.position_in_unique_constraint
    		and y.constraint_name = c.unique_constraint_name
		order by c.constraint_name, x.ordinal_position;

Show active connections
-----------------------
	SELECT count(*) 
	FROM pg_stat_activity;
