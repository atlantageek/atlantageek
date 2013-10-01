---
layout: post
title: "Why multi-field indexes in postgresql still matter."
description: ""
category: 
tags: []
date: 2013-02-08 14:49:51 -04:00
categories: 
- soapbox
tags: []

---

## Why wouldn't it.
As of 8.1 postgresql includes the concept of a bitmap heap. A bitmap heap as implemented in postgresql is an array/matrix of bits stored in ram with each cell representing a record in the table being queried. Using the conditions included in the query the cell for each record is activated or deactivated. to show that these records should be included in the result of the query's search.  At the end of the query the bitmap heap is what determines wich records will be shown.  This bitmap can be built by scanning different indexes based on the query criteria 
Compare this to how simpler databases like mysql work. In mysql only a single index is used for each table in each query.

Let's try this with a basic database.  Below is a list of sales records.

###SALES

	# |Product|Date      |Store|Count|
	1 |laptop |2-05-2013 |ST1  |10   |
	2 |laptop |2-06-2013 |ST1  |20   |
	3 |laptop |2-07-2013 |ST1  |30   |
	4 |laptop |2-05-2013 |ST2  |20   |
	5 |laptop |2-06-2013 |ST2  |30   |
	6 |laptop |2-07-2013 |ST2  |10   |
	7 |laptop |2-05-2013 |ST3  |20   |
	8 |laptop |2-06-2013 |ST3  |30   |
	9 |laptop |2-07-2013 |ST3  |10   |
	10|phone  |2-05-2013 |ST1  |10   |
	11|phone  |2-06-2013 |ST1  |20   |
	12|phone  |2-07-2013 |ST1  |30   |
	13|phone  |2-05-2013 |ST2  |20   |
	14|phone  |2-06-2013 |ST2  |30   |
	15|phone  |2-07-2013 |ST2  |10   |
	16|phone  |2-05-2013 |ST3  |20   |
	17|phone  |2-06-2013 |ST3  |30   |
	18|phone  |2-07-2013 |ST3  |10   |

Now consider the following query:
select count(\*) from sales where product = 'laptop' and Date > '02-06-2013' order by Date
To keep from accessing records that do not fit the query then both product and date must be in the same index. If the queries are very ad hoc then multiple indexes possibly of each combination would need to be created. And these indexes would have large records because 2 or 3 attributes would be included.  And if you included a fourth attribute then you will soon hit 7-8 indexes to cover every combination.

Now compare this to the postgresql approach 
Instead you would create a single attribute index for each searchable criteria. Then for each condition in the where portion of the query it will fill in the bitmap array.  
So for the above query here are the two results of the index query.

	product = laptop
	Record Positions:    1 |2 |3 | 4| 5| 6| 7| 8| 9|10|11|12|13|14|15|16|17|18
	product = 'laptop' :  1  1  1  1  1  1  1  1  1  0  0  0  0  0  0  0  0  0
	Date > '02-07-2013':  0  0  1  0  0  1  0  0  1  0  0  1  0  0  1  0  0  1
	AND Operation      :  0  0  1  0  0  1  0  0  1  0  0  0  0  0  0  0  0  0

Using the combined bitmap the system knows which records to pull.

This is faster than a scan and more efficient than a large number of indexes.  However it is still not as fast as a combined index. based on product and date.

So consider a different scenario.  So far you have created an index on every attribute on your mult-tenant application. You sale your application to corporate offices that include multiple stores.  So rarely do you have a query that does not have some sort of store criteria. You may even have considered partitioning your application by groups of stores to improve your performance. Before you do this consider a different alternative.  For all your single attribute indexes prefix the store ID. You will still use the bitmap heap when doing searches but when building the bitmap heap you will filter your queries to a greater extent.

The take away from this is that the single attribute indexes are good to have in your database but that does not mean that that is the only type of indexing you need.  For slow queries combined indexes could be faster.
