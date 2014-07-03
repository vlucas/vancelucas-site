---
title: Titanium Proxy Objects With JavaScript Call and Apply
date: 2014-07-03 03:52 UTC
tags: ['titanium', 'mobile', 'javascript']
---

Sometimes language and platform abstractions bite you. They can seem pretty
straightforward and "just like the real thing", but sometimes they have odd
behaviors that don't quite work the way you expect.

## Dynamic Programming with .call() and .apply()

The `.call()` and `.apply()` functions are something you will use a lot if your
code involves a lot of dynamic arguments, such as building an array of
arguments that then need to be mapped to actual ordered function arguments in a
function or method call.

Since Titanium runs native JavaScript with the V8 engine bundled in your app,
you will naturally find yourself attempting things like this:

```javascript
var args = [sql].concat(params);
var db = Ti.Database.open('dbname');
var rs = db.execute.apply(db, args);
```

But if you run that, and you will be presented with the head-scratching error:

```
invalid method 'execute:'
```

Although the error is pretty confusing - you _know_ there is a method named
_execute_ on the `db` object - there actually is a good explanation for this
behavior.

## Titanium Proxy Objects

What intiutively seems to you like a normal JavaScript variable here - `db` -
is actually a [proxy
object](http://www.appcelerator.com/blog/2012/02/what-is-a-titanium-proxy-object/)
created and returned by Titanium with the `Ti.Database.open` call. Titanium
also returns proxy objects for pretty much every other component that has a
native counterpart they are bridging over to behind the scenes. This black
magic is fundamental to how Titanium works internally.

## The Solution

Although the main Titanium [proxy
object
explanation](http://www.appcelerator.com/blog/2012/02/what-is-a-titanium-proxy-object/)
suggests creating a wrapper object for the method call, that's actually not necessary
at all, and creates a lot of extra work, as well as many additional lines of code.

The best solution is to use a bit of JavaScript meta programming and call the
`apply` method directly from the `Function` prototype:

```javascript
var args = [sql].concat(params);
var db = Ti.Database.open('dbname');
var rs = Function.prototype.apply.call(db.execute, db, args);
```

This is a nice one-liner that achieves the same end result, is much easier to
undertand, and does not require a wrapper object or another layer of
indirection to achieve.



