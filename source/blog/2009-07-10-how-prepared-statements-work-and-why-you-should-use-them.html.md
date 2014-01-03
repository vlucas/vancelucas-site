---
title: 'How Prepared Statements Work, and Why You Should Use Them'
date: 2009-07-10
tags: mysql, php
published: false
---

Prepared statements differ from normal queries in one major way: Instead of
sending one SQL string with the values defined by single quotes in one package,
the SQL string and values are sent in two separate calls. Prepared
statements themselves are like query blueprints, with placeholders
where the values will go. The query values are sent in a separate call
with a reference to the prepared statement, and are dropped in place
and executed.

This post approaches the problem and presents a solution from a PHP angle, but
even if you're not a PHP developer, you should be able to follow along with
some creative substitution.

The Old Way

Prepared statements take longer because they are 2 round-trips to the database
for a single query. As soon as you prepare a query, it's sent to the database
with the placeholders you set. So the database engine takes that prepared
statement and maps out the query and optimizes it for execution. Then when you
call execute() only the values you give are sent to the database, with a
reference to that query you just prepared. The database engine drops in the
values and runs the query. This is totally immune to SQL injection, because the
database engine already knows exactly where the values begin and end (the
    placeholder marker(s) you set), and therefore never need escaping. The
reason SQL injection exists in the first place is because the entire query is
interpreted upon execution, values and all. So if anything interferes with the
quotes surrounding your values, the engine thinks that value has ended and thus
a security hold is introduced. That problem is avoided all together with
prepared statements by letting the database engine know ahead of time exactly
where to put each value you pass to it later on. There is no need for escaping
and there is no need to worry.
