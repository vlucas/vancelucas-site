---
title: 'MySQL Series: How to Detect UTF-8 and Multi-byte Characters'
date: 2010-03-24
tags: database, i18n, mysql, mysql, sql, utf8
---

Multi-byte characters can cause quite a few headaches for the unsuspecting
webmaster. Sometimes all you need to do to figure out how to fix the problem is
detect which database records have UTF-8 data in them and which ones do not. If
you've been scanning records manually, stop now. Here's a quick query to cure
your ales:

Return all the rows with multi-byte characters in table posts on field title:

```sql
SELECT *
FROM posts
WHERE LENGTH(title)  CHAR_LENGTH(title)
```

This does pretty much what it looks like: returns all the rows in the posts
table where the title's character length does not match the title's length.
This works because the `LENGTH` function returns the number of bytes in the
string, while the `CHAR_LENGTH` function returns the number of characters in the
string. If the string contains multi-byte characters (individual characters
that are made up of more than one byte) like international UTF-8
characters, the two functions will return different numbers and will not be
equal, thus including that row in your results.
