---
title: 'PHP array_merge - Preserving Numeric Keys'
date: 2009-01-06
tags: general, php
---

This is just a quick post on PHP's default behavior of re-indexing numeric keys when using PHP's internal 
[array_merge](http://www.php.net/manual/en/function.array-merge.php) and 
[array_merge_recursive](http://www.php.net/manual/en/function.array-merge-recursive.php) functions, because it's a problem I recently ran into, and was unable to find a quick solution to online.

Basically, the problem is that if you're using numerically-indexed arrays with a set number that you don't want to change (like an ID or some other unique identifier), you can't use array_merge, because it automatically re-indexes all the numeric keys in the array to start with 0 on down in order.  There is no flag or option for the function to NOT do this, but there is another way to achieve the same result using PHP's little-documented overridden plus operator '+' for appending an array to another array.

So just replace this:$destinationArray = array_merge($array1, $array2);

With this instead:

$destinationArray = $array1 + $array2;

Both $array1 and $array2 MUST be arrays or a fatal error will be thrown, so you may want to do some type checking or casting before that line of code.  The difference is that instead of merging the arrays together, the second array will simply be appended to the first one with no changes.

Note that the plus operator for arrays '+' is only one-dimensional, and is only suitable for simple arrays. If you need a multi-dimensional or complex solution, 
[Keith Devens has a custom merge function](http://keithdevens.com/weblog/archive/2003/Mar/30/MergeTwoArrays) that might work for you.