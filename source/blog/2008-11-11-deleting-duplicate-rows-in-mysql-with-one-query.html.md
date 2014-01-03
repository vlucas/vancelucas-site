---
title: 'Deleting Duplicate Rows in MySQL With ONE Query'
date: 2008-11-11
tags: mysql
published: false
---

So you have a MySQL table that, somehow or another (usually on many-to-many relation tables), winds up having rows with duplicate data.  Trouble is, finding and deleting these rows most of the time
[involves several steps](http://www.justin-cook.com/wp/2006/12/12/remove-duplicate-entries-rows-a-mysql-database-table/) that could leave your website with a few errors showing here and here if you don't perform the updates fast enough.  So is deleting multiple duplicate rows in mysql
without deleting the first original row possible with just ONE query?  Turns out, it is with subqueries, a MySQL feature available since the 4.1 branch.  The steps are bit tricky and the query is complex, so I've broken it down into several steps that should make it easier to follow.###Which records are duplicates?

The first step is to get a list of all the records that are duplicates, and you
really should not skip this step just so you can know - for sure - which records will be deleted when the final query is put together at the end.



SELECT COUNT(*) AS icount
FROM categories
GROUP BY name
HAVING icount > 1
This SHOULD work (and hopefully will in future versions of MySQL):




DELETE FROM modules_settings
WHERE modules_settings.id IN
(
  SELECT id
  FROM modules_settings AS ms
  WHERE name = 'module_sharing' AND
  (
    SELECT COUNT(*) AS icount
    FROM modules_settings
    WHERE module_id = ms.module_id AND name = 'module_sharing'
    GROUP BY module_id
    HAVING icount > 1
  ) > 1
  LIMIT 1, 9999) AS RetardedlyNecessaryAdditionalSubquery
)
But is DOESN'T.  It ends up producing this error:

ERROR 1235 (ER_NOT_SUPPORTED_YET)
SQLSTATE = 42000
Message = "This version of MySQL does not yet support
'LIMIT & IN/ALL/ANY/SOME subquery'"
The issue is outlined in the
[MySQL subquery errors manual page](http://dev.mysql.com/doc/refman/5.0/en/subquery-errors.html).

Luckily, there is a workaround that's not too difficult to do.  The solution is to add an additional subquery around the query with the LIMIT clause, placing the query inside the FROM clause of your new subquery:



DELETE FROM table
WHERE table.id IN
(SELECT * FROM(
  SELECT id
  FROM table AS tbl
  WHERE tlb.column = 'duplicated_value' AND
  (
    SELECT COUNT(*) AS icount
    FROM table
    WHERE column = 'duplicated_value'
    GROUP BY id
    HAVING icount > 1
  ) > 1
  LIMIT 1, 9999) AS RetardedlyNecessaryAdditionalSubquery
)
