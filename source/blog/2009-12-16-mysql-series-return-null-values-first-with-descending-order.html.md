---
title: 'MySQL Series: Return NULL Values First With Descending Order'
date: 2009-12-16
tags: database, database, mysql, mysql
---

Sometimes there are unique situations where you need to order query results by a particular field in descending order, but also need NULL values first. The default (and logical) behavior of MySQL in this case is to return NULL values last, because in descending order they have the lowest value (none). But what if you really need to reverse this and force NULL values to the top of the result set?

more
I recently ran into this situation with movie results. The business requirement was that the movies be displayed in descending order by release date, so that the newest ones appeared first. Okay, pretty simple - until I looked at the results. Turns out, we had data on movies that were currently in production, but did not yet have a release date set. I accounted for this earlier by allowing the release date column to be NULL in the table schema, but forgot about the ramifications of that on my queries. These movies were obviously newer, but were always displayed last on the results page because of their NULL release dates. Sounds like the perfect scenario for a conditional statement.

Using the 
[MySQL Control Flow Functions](http://dev.mysql.com/doc/refman/5.0/en/control-flow-functions.html), I quickly crafted an IF() statement in the SELECT portion of the query that looked like this:IF(m.date_released IS NULL OR m.date_released = '0000-00-00', 1, 0) AS in_production

So if the release date has a NULL value or has been inserted as a blank string and formatted to '0000-00-00', it is given a value of 1, and 0 otherwise. We then sort descending by our new 'in_production' alias in the ORDER BY clause to get the NULL or empty values on top (assigned a 1 value), and then by release date and other criteria second.

The whole query looks something like this:


SELECT m.*, IF(m.date_released IS NULL OR m.date_released = '0000-00-00', 1, 0) AS in_production
FROM movies AS m
ORDER BY in_production DESC, m.date_released DESC

It's an easy way to manipulate the result set in MySQL without slowing down the query or issuing multiple queries.