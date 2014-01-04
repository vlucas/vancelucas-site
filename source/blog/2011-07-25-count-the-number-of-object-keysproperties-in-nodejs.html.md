---
title: 'Count the Number of Object keys/properties in Node.js'
date: 2011-07-25
tags: general, javascript, node-js, nodejs, programming
---

When using the excellent
[formidable](https://github.com/felixge/node-formidable) library to handle file
uploads, I needed to get a count of the number of files unloaded in a
multi-part form. Javascript arrays have a .length property that you can use,
  but objects do not. I instinctively typed:files.length

Which returned undefined. So if there is no length property present, an easy
way to count the number of keys or properties of an object in ES5-compliant
javascript environments like [Node.js](http://nodejs.org) is to use the Object
prototype directly:

```javascript
Object.keys(files).length
```

A little more typing, but it is fast, efficient, and most importantly: already built-in.
