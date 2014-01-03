---
title: 'Why MVC is so Popular on the Web'
date: 2009-06-17
tags: design, design-pattern, general, mvc, php
published: false
---

The MVC design pattern has been getting a lot of attention in the past few
years.  It seems like a new MVC framework pops up every week, and discussions
about MVC have become commonplace throughout web programming communities.###MVC
is Here to Stay

While MVC is certainly the "new thing" again (funny, because it actually [dates
 back to 1979](http://en.wikipedia.org/wiki/Model-view-controller#History)),
it won't be a passing fad, and it won't fade quickly - at least not on the web.

### Solving the Fundamental Problem of Web Development

Unlike most dektop applications which can be completely coded with one or two
different programming languages, websites are a mix of several different
programming languages that are constantly changing.  The typical website or web
application in composed of no less than 6 different programming languages:

* Server-side language like PHP/Ruby/Python/.NET, plus:
* SQL - (Variants: MySQL, MSSQL, PgSQL, SQLite)
* HTML/XHTML
* CSS
* JavaScript
* XML (Plus specific formats like RSS and Atom and possibly XSLT)
* JSON if the web application has an API
* And who knows what else a few years from now...

As a result, the code naturally has to be separated somehow, and the same
content has to be able to be displayed in many different formats (most commonly
HTML plus XML and JSON for APIs).
