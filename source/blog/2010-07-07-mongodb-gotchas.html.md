---
title: 'MongoDB Gotchas'
date: 2010-07-07
tags: database, databases, mongodb, nosql, nosql, programming
---

Most developers are coming from a background with relational database-specific
experience, and then trying out some new NoSQL databases like
[MongoDB](http://mongodb.org). Here are some "gotchas" I ran into while using
MongoDB with my MySQL hat still on.

READMORE

### Queries are case-sensitive

Fields and queries in MongoDB are case-sensitive:

```javascript
var test1 = db.test.find({'tags': 'jquery'}).count();
var test2 = db.test.find({'tags': 'jQuery'}).count();
test1 == test2; // Output is false - they do not query for the same information
```

This can cause some headaches if you don't normalize user input ahead of time.
Imagine you already have several posts tagged with 'NoSQL' and users enter
'nosql' in a tag search box. If you don't normalize how the data is stored
internally (like lowercase all tags), your user will see a much smaller set of
posts than they are expecting to see. This is something you don't have to worry
or even think about with MySQL and most other relational databases.

If you can't normalize the stored data, but still want a case-insensitive
query, then you must perform a slower [regular expression
query](http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-RegularExpressions):

```javascript
db.test.find({'tags': /jquery/i}); // Note the 'i' flag for case-insensitive
```

### Data is type-sensitive

Data stored within MongoDB knows it's type. There is a small but significant
difference between these two records:

```javascript
{'count': 102}; // 'count' is stored as an int
{'count': "102"}; // 'count' is stored as a string
```

This is obvious to any programmer when presented like this, but what may not be
obvious is that this also affects how you can query for these records:

```javascript
// This returns 1 instead of 2, because it only matches the integer value
db.test.find({'count': 102}).count();
```

This is due to how MongoDB stores documents internally with
[BSON](http://bsonspec.org/) (Binary JSON), using various [MongoDB data
types](http://www.mongodb.org/display/DOCS/Data+Types+and+Conventions). This
means - like the point above - that you must pay attention to how you are
saving data into MongoDB, because it will affect how you can query for it
later.

### Documents sizes are capped at 4MB each

This isn't a big issue for most people, but it's something to be aware of if
you plan on storing large chunks of text or nesting a bunch of objects inside a
single document.  [Nesting comments inside article
documents](http://www.businessinsider.com/how-we-use-mongodb-2009-11) is a
particular approach that may give you pause knowing there is an upper threshold
on document size.

Note that this limitation is not an issue for storing files in MongoDB because
[GridFS](http://www.mongodb.org/display/DOCS/GridFS) transparently divides the
contents of files larger than 4MB across multiple documents.


### Only one index is used per query

Simply adding more indexes doesn't always help queries run faster. MongoDB
can't use multiple indexes together like MySQL and other RDBMS can (see "
[Index Merge
Optimization](http://dev.mysql.com/doc/refman/5.0/en/index-merge-optimization.html)").
This means that if a query is selecting or sorting based on multiple fields, a
compound index should be created with those fields for the query to run most
efficiently.

### MongoDB is for high-memory and 64-bit systems only

Although MongoDB can be downloaded, compiled, and installed on 32-bit systems,
it should never be run in production on them. This is because [MongoDB
stores all indexes in
memory](http://www.mongodb.org/display/DOCS/Indexing+Advice+and+FAQ#IndexingAdviceandFAQ-MakesureyourindexescanfitinRAM.)
as well as [memory-mapped
files](http://www.mongodb.org/display/DOCS/Caching) for all disk I/O
for increased speed and throughput. While these two facts aren't
"gotchas" themselves - they are clearly explained on the website - it
does mean that MongoDB can quickly use a lot of memory, especially as
the size of your database grows and becomes significant. Since 32-bit
systems have an effective [3GB memory
limit](http://en.wikipedia.org/wiki/3_GB_barrier) of addressable
memory space, they prevent you from being able to add more memory as
the size of your database grows. This is also something to think about
if you plan to run MongoDB on a small VPS with limited memory, even if
it is 64-bit. You may be forced into a memory upgrade sooner than you
are prepared for if your database is large or rapidly growing.

### Subtle Differences

When starting out with MongoDB, it's easy to draw a lot of parallels to MySQL
and make a lot of assumptions based on those similarities. Collections are kind
of like tables, fields are kind of like columns, a document is kind of like a
row. You may find yourself going on to make more assumptions about how it's
storing data, how you can query it, what you can do with it, etc. without even
knowing it. So while MongoDB has a lot of similar features and is one of the
easiest NoSQL databases to transition to from a relational database, it has
some very distinct differences "under the hood" that you have to be aware of
and plan for ahead of time. The [MongoDB
documentation](http://www.mongodb.org/display/DOCS/Manual) is well-written, and
is pretty good at explaining any potential differences or "gotchas", so make
sure to read it thoroughly before making the jump - and watch that first step.

