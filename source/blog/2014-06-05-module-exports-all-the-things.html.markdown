---
title: '<code>module.exports</code> All The Things!'
date: 2014-06-05 04:38 UTC
tags: ['javascript', 'commonjs', 'module', 'nodejs']
---

One thing I find myself doing a lot in javascript files for
[node.js](http://nodejs.org), or any other
[CommonJS](http://wiki.commonjs.org/wiki/CommonJS)-based system is typing
`module.exports` quite a lot, or creating these obnoxiously large and
repetitive `module.exports` declarations at the bottom of my files.

For example in a helper library for a
[Titanium](http://www.appcelerator.com/titanium/) mobile project, I had the
following code snippet:

```javascript
// ...

// Platform detection helpers
var osname = Ti.Platform.osname;
var isiPhone = function() {
  return ('iphone' == osname);
};
var isiPad = function() {
  return ('ipad' == osname);
};
var isiOS = function() {
  return (isiPhone() || isiPad() || 'ios' == osname);
};
var isAndroid = function() {
  return ('android' == osname);
};

// ... (lots of other functions) ...

module.exports = {
  // ...
  isiOS: isiOS,
  isiPhone: isiPhone,
  isiPad: isiPad,
  isAndroid: isAndroid,
  // ...
}
```

This is a small sampling, but there were many, many other helpful functions in
that file. You can probably imagine the number or times I had to type out all
the variable names - _twice!_ That seems like such a waste of typing. The other
problem with this approach is that there is nothing visually signaling to me
what is going to be a part of the public API, and what is private and internal.
Everything is just a `var`, and I have to recall what I want exposed when doing
the `module.exports` call at the end.

To avoid these issues, I could just prefix everything with `module.exports`
directly, but that's a lot of typing too - and doing that makes it cumbersome
to use a function in another place in the same module file.  Since there would
be no local variable assigned, I would have to type the full
`module.exports.functionName()` every time to call another function within my
own module with this approach.

## A Better Solution - Namespacing

The way I have come up with to solve this in my own code is to create a single
empty object literal at the top of the file named `ns` (for _namespace_). I
then create functions on the `ns` object as I go, and export all the properties
on the `ns` object at the end of the file.

So in my original example, `var isiPhone = ...` becomes `ns.isiPhone: ...` -
shedding a few keystrokes _and_ gaining more clarity, because I know that
everything in the `ns` object gets automatically exported at the end of the
file:

```javascript
var ns = {};

// ...

// Platform detection helpers
var osname = Ti.Platform.osname;
ns.isiPhone = function() {
  return ('iphone' == osname);
};
ns.isiPad = function() {
  return ('ipad' == osname);
};
ns.isiOS = function() {
  return (ns.isiPhone() || ns.isiPad() || 'ios' == osname);
};
ns.isAndroid = function() {
  return ('android' == osname);
};

// ...

// Export all the things! *\('o')|
for(prop in ns) {
  if(ns.hasOwnProperty(prop)) {
    module.exports[prop] = ns[prop];
  }
}
```

You could argue that any saved keystrokes are negated with the `module.exports`
loop at the bottom, but the real benefit is the added clarity - I now know that
`var osname` is just a temporary placeholder for data, and will never be in the
public API, and that all the functions following it will be. I don't have to
scroll to the bottom or go anywhere else - I know this immediately by looking
at them, and re-using the public functions in `ns.isiOS` isn't painful. Besides,
I copy and paste the for loop anyways :).

*UPDATE:* You may wonder "why not just assign `module.exports` to `ns`, like
this: `module.exports = ns;`?" The reason is that using the `hasOwnProperty`
method ensures that only properties you have explicitly added get exported. In
this case, it may not make a difference since we're using a bare object
literal, but it's good practice to use, so I do. Feel free to save a few lines
of code with the assignment if you feel differently!

