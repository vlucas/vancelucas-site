---
title: 'The One Character Block Comment'
date: 2009-07-21
tags: code-comments, comments, javascript, php, php, programming, programming
---

When debugging, I often find that I have to comment and un-comment a block of
code several times during the process of trying to find out what's going on.
That used to mean typing and deleting comment block characters repetitively,
but not anymore. Here's a simple solution to that problem: Comment or
un-comment an entire code block of code by typing or deleting a single
character.

I was able to arrive at this solution by combining the one-line comment with
the comment block in a way that takes advantage of the rules the different
types of comments have to follow.

READMORE

### The One-Line Comment

One-line comment rules dictate that everything after the comment characters
must be ignored for the rest of that line. They look like this:

```php
<?php
// My code below
$someVar = array("foo", "bar", "blah");
echo "
";
print_r($someVar);
echo "";
```

### The Block Comment

Block comment rules dictate that once the beginning characters are started,
everything up until the ending characters is ignored. They look like
this:

```php
<?php
/* My code below
$someVar = array("foo", "bar", "blah");
echo "
";
print_r($someVar);
echo "";
*/
```


### Combining The Two: The Best of Both Worlds

Using both those comment style rules, we can combine the two comment styles in
a way that will allow us to comment and un-comment a block of code with by
adding or deleting a single character. The trick is to make both the beginning
and ending lines both single-line AND block style comments, like so:

```php
<?php
//* My code below
$someVar = array("foo", "bar", "blah");
echo "
";
print_r($someVar);
echo "";
// */
```
In the above example, you notice that the code will still run - it's not
commented out because the first line is following the rules of the single-line
comment - ignoring the block comment declaration on the same line. By removing
the first backslash at the beginning of the block, we can comment out the
entire code block by enabling the block comment style instead of the
single-line style. The last single-comment line is ignored because the block
comment rules ignore everything up until the end block comment declaration at
the end of the line:

```php
<?php
/* My code below
$someVar = array("foo", "bar", "blah");
echo "
";
print_r($someVar);
echo "";
// */
```

So a single character - a backslash at the very beginning of the block
declaration can now comment or un-comment the entire block by switching the
comment rules back and forth between single-line and block style. The code
examples provided are in PHP, but this trick will work with any language that
supports both single-line and block style comments.

