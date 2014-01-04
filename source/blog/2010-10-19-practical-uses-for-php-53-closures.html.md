---
title: 'Practical Uses for PHP 5.3 Closures'
date: 2010-10-19
tags: closures, late-static-binding, namespaces, php, php, php-5-3, programming
---

Closures are a new language-level feature that has been added to php 5.3, along
with namespaces, late static binding, and a slew of other new features,
patches, and updates. If you're like me, you might be wondering what the
practical uses for these new features are before you can rightly justify
diving in and using them in new or existing projects. I experimented a lot
with closures and possible uses over the past few weeks, and came up with
some compelling reasons to start using them.

READMORE

### Templating

Templating is perhaps the most compelling and creative use of closures that I
have found, yet I haven't seen any people or projects using closures this way.
Closures are an excellent way to allow custom formatting for re-useable
template widgets, like forms, datagrids, and lists. Re-useable forms and
datagrids are pretty common in frameworks, because no one likes the monotony of
creating a bunch of HTML forms or tables. They all seem to work great, until
you have to change a few things in how they output the input fields or format
cell data - then they can get a bit hairy and ugly to work with, and often
require inheritance or use some obscure template syntax of their own with
tokens that get string replaced and weak-to-no support for basic control
structures like loops and if statements. Fabien Potencier published a
[proof-of-concept for individual field
templates](http://groups.google.com/group/symfony-devs/browse_thread/thread/fb5fb9732cf5fc1e?pli=1)
in Symfony2. This approach, while much more flexible than string-replaced field
tokens, adds quite a bit of overhead because you now have to render one
template per input field to produce a complete form. The same end result can
now be easily achieved with the use of closures:

```php
<?php
$table = new \Alloy\View\Generic\Datagrid('datagrid');
$table-data($this->posts)
    ->column('Post', function($item, $view) { return $item->title; })
    ->column('Status', function($item, $view) { return $item->status; })
    ->column('Date Published', function($item, $view) {
        return ($item->date_published) ? $view->dateFormat($item->date_published) : '(Unpublished)';
    });
echo $table;
?>
```

In this example, our generic "Datagrid" view accepts `$this->posts` as its data
source input, and loops over it with a foreach internally. It then implicitly
passes two arguments to any closure given that defines column output - `$item`
- the current item in the posts collection we provided, and `$view` - the view
object itself so helpers and other typical functions can be accessed.

The added benefits for using closures in this context are enormous. There is no
additional (and limiting) pseudo-syntax to parse, and no additional item
templates to render. The custom content is overridden in-line right where you
want it to be (instead of being one-step removed in a separate template), and
you can still use the native PHP code you know and love. The provided example
is from [Cont-xt CMS](http://cont-xt.com) built using my own [Alloy HMVC
Framework](http://alloyframework.org).

Imagine how powerful and simple something like
[Zend_Navigation](http://framework.zend.com/manual/en/zend.navigation.html) and
the [resulting navigation view
helpers](http://framework.zend.com/manual/en/zend.view.helpers.html#zend.view.helpers.initial.navigation)
would become with support for closure templates. You would no longer have to
rely on a deep hierarchy of nested configuration arrays, custom partial
templates, or complex object inter-dependencies to generate page links with
your own custom code.

### Dynamic Code Extension

Dynamically extending a class and modifying the behavior of a code block with
the use of closures is the foundation of a new php 5.3 framework called
[Lithium](http://lithify.me/). Lithium's specific approach uses closure chains
inside function bodies to allow end-users to modify core functionality without
modifying the core files themselves or having to use inheritance.

A more extreme example uses closures to enable dynamic run-time object
creation. The idea is in an article titled [PHP Object Oriented Programming
Reinvented](http://dhotson.tumblr.com/post/1167021666/php-object-oriented-programming-reinvented).
The entire object and all its functionality is created with closures. The
article is a good demonstration of the power of closures, but has little
practical use.

One of the more practical (and perhaps controversial) uses I came up with is
replacing string configuration values with closures if the return values needs
to be dynamic. Here's an example:

```php
<?php
// Set in base config file
$cfg['path_uploads'] = function($cfg) { return "/uploads/" . $cfg['user_id'] . "/"; };

// Set dynamically by database value from logged-in user later in your application
$cfg['user_id'] = 74;
?
```

This way, the `path_uploads` config value will not have to be changed when the
`user_id` config value is. The closure will cause the config value to always be
evaluated when called, and will always use the current `user_id` value, so it
will never be "out of date". This, of course, requires you to have a standard
way to retrieve config values from named keys, because closures can't just be
automatically converted to a string by themselves, and do have to be called a
different way (like functions).

This is the only use that comes with a warning: beware how far down this road
you travel. Dynamic code extension provides a lot of flexibility, but comes
with the downside of code obfuscation and performance penalties. You are
essentially adding class methods and modifying code behavior in places other
than the code itself, which can lead to confusion when trying to trace code
execution. Stack traces and development toolbars can help here, and using an
opcode cache like APC can minimize the performance penalty.

### Delayed Execution

Closures are a great tool to use anywhere you need to delay PHP code execution
until a later time. A great example of this use is for a job queue system with
multiple workers. Using closures, you can execute any arbitrary PHP code you
need to without conforming to a specific structure -- there is no need to
create new classes or functions with set names determined by naming
conventions, and no need to put "worker classes" in any specific place in the
filesystem. Your worker task can be passed around and executed in parallel at
will by the job queue.

Example: [PHP Native Job Queue](http://github.com/kore/njq).

### Caching

Using closures for caching is a bit of an extension of the delayed execution
idea. Consider a cached block of code inside a view template using a closure:

```php
<?php echo $this->cache(function($view) { ?>
    <?php
    // Expensive external HTTP request
    $blogRss = $view->helper('feedReader')->fetch('http://github.com/zendframework/zf2/commits/master.atom');
    $widget = new \MyApp\View\Generic\FeedList('feedlist');
    $widget->feedContent($blogRss, 'atom');
    return $widget;
    ?>
  <?php }, 'github_zf2_commits', 3600); ?>
```

Using a closure here offers two advantages over the typical `beginCache`,
`...`, `endCache` type calls that are more prevalent in pre-5.3 code. The
first benefit is that through the use of a closure, our code block is
contained in a variable before it is even executed. This allows us to use
a cache backend like Memcache without having to use output buffering at
all, saving us a tiny bit of overhead. The second big benefit is that
executing our code chunk is entirely optional, without manually checking
the cache first. This second benefit could also allow you to disable
rendering of certain cached content completely if you need it, without
any code changes in your view templates. Heavy-load or traffic spike
scenarios come to mind as possible candidates for this functionality.
Imagine being able to make a small configuration change to skip certain
code blocks like `$cache->disable('github_zf2_commits')` to serve a "lite"
version of your site when you need all the server resources you can get.

### Convenience

This may sound like a bit of a cop-out reason, but using closures in some cases
really does make things a lot easier, and truly better. Consider the case where
you have to use a one-off function to apply some sort of custom filtering logic
to an array or collection of objects. With closures, you can just specify the
logic in-place right where you need it instead of having to create a new
function for the job in a separate location and having to figure out where to
put it in your application, and then how to call it. It produces the same
result in fewer lines of code with no side-effects (like having to make a
global function).

```php
<?php
// Collection of entities
$objCollection = array(
    new Entity(),
    new Entity(),
    new EntityRelation(),
    new OtherRandomJunk()
);

// Get only Entity objects
$entitiesOnly = array_filter($objCollection, function($val) {
    return ($val instanceof Entity);
});
```

In the above example, it may not make sense to create a separate named or
global function for this collection filtering, because it may only ever be used
in one place. If this is the case, it makes sense to use a closure, and makes
the code more concise and readable because everything is all in one place.


###Summary


Closures are a game-changer for PHP all by themselves. Throw namespaces and late static binding in the mix as well, and PHP 5.3 reaches a level of power and dynamic flexibility that PHP has never been able to reach before. If you have been sitting on the sidelines, stuck in PHP 5.2 land - you desperately need to upgrade. This is the future of PHP.
