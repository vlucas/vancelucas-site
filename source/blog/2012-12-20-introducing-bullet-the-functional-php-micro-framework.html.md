---
title: 'Introducing Bullet: The Functional PHP Micro-Framework'
date: 2012-12-20
tags: framework, functional, micro-framework, php, php, programming
---

[Bullet](http://bulletphp.com) is a new PHP micro-framework with a unique
functional approach to URL routing that allows for more flexibility and
requires less verbosity than the more typical full route+callback approach
found in other micro-frameworks.

###The Problem with Independent Scope

The main problem with most micro-frameworks and even full-stack MVC frameworks
that leads to code duplication is that the callback or method executed to
perform the action and respond to the URL route lives fully within its own
scope. This means that you are forced to repeat a lot of setup code across URL
route handlers that load the same resource, authorize it, etc.

Some typical micro-framework code might look like this:

```php
<?php
// View single post
$app->get('/posts/:id', function($id) {
    $post = Post::find($id);
    check_user_acl_for($post);
    // ...
});

// Delete post
$app->delete('/posts/:id', function($id) {
    $post = Post::find($id);
    check_user_acl_for($post);
    $post->delete();
    // ...
});

// Edit post
$app->get('/posts/:id/edit', function($id) {
    $post = Post::find($id);
    check_user_acl_for($post);
    // ...
});
```

You may be able to move the ACL check to a middleware layer or "before" hook if
the framework supports it, but there is always a certain amount of duplicate
code you will either never be able to get rid of, or have to jump through hoops
to get rid of (like adding more abstraction or re-checking the current URL,
etc).


###The Benefits of Shared Scope

Bullet uses a unique nested callback style that splits the URL by directory
separator and only handles a single part of the URL at a time with it's own
callback. At first blush, this approach might seem like more work, but the key
to how Bullet works is that nested closures - by definition - can use variables
defined in the scope of their parent. This leads to some pretty powerful and
profund capabilities that can only be done using the same nested closure style
that Bullet uses.

READMORE

```php
<?php
$app->path('posts', function($req) use($app) {
    $app->param('int', function($req, $id) use($app) {
        $post = Post::find($id);
        check_user_acl_for($post);

        // View (GET)
        $app->get(function($req) use($app, $post) {
            // ...
        });

        // Delete
        $app->delete(function($req) use($app, $post) {
            $post->delete();
            // ...
        });

        // Edit ('edit' path added after id)
        $app->path('edit', function($req) use($app, $post) {
            // ...
        });
    });
});
```

Notice in the example here how the code to load the desired post and perform
the ACL check only have to be run ONCE. Any other code or URL routes below that
point will automatically be safe and can use($post) to get access to the
already loaded Post object.

### Other Advantages and Positive Side-Effects

Since Bullet's URL routes handle only a single path segment at a time and are
relative to the parent execution scope, it opens up all kinds of possibilities
for code re-use unimaginable in most other PHP frameworks today. The first and
perhaps most obvious one is that URL routes can be crafted however you want,
and can be nested unlimited levels deep with no restrictions beyond
your imagination. The second one is that it becomes trivially easy to
do things that are inexplicably difficult with other frameworks like
create a base version folder for an API like "v1" or "v2" that then
includes the other main paths below it like "v1/posts" and "v2/events".

Perhaps the most significant benefit of this approach is that if you logically
separate your routes into different include files ('posts.php', 'events.php',
'comments.php', etc.), you can include them inside other route handlers,
 and since both PHP includes and closures are context-sensitive, they
 will work perfectly and will act as nested routes from whatever path
 you include them in. Bullet even has a built-in url method that helps
 build context-sensitive URLs that can be dynamically nested in-context
 from the current URL path.

The classic use-case for this nesting functionality is an 'admin' path:

```php
<?php
$app->path('admin', function($req) use($app) {
    some_acl_check_to_ensure_admin_that_throws_exception_if_not();

    require 'posts.php'; // For /admin/posts ...
    require 'events.php'; // For /admin/events ...
    require 'comments.php'; // For /admin/comments ...
});
```

A lot of other frameworks - if they can even support doing this - use
additional concepts like route namespaces to solve this problem. With Bullet,
 the usage is simple and straightforward, and the logic is simple and
 easy to understand - this is how bullet already works, so there are
 no new concepts in play. There are no routing tables or
 pre-determined rules that make this impossible to do, and the
 concepts here are all native to PHP and fully leverage how PHP
 already works - It's just one more path to declare.

### Polymorphic Code Re-Use

Taking this logic even further, you can create a file with routes that are
intended for polymorphic-style code re-use, like allowing 'comments.php' to be
nested within any other path - in our case, both 'posts' and 'events'. Such use
might look something like this:

```php
<?php
$app->path('posts', function($req) use($app) {
    $app->param('int', function($req, $id) use($app) {
        $post = Post::find($id);
        check_user_acl_for($post);

        // View (GET)
        $app->get(function($req) use($app, $post) {
            // ...

            // Use array notation for variable passing on the $app instance
            // Tell comments to load comments for post_id
            $app['comments'] = array('type' => 'post', 'type_id' => $post->id);

            // Include our nested comments
            require 'comments.php'; // will be 'posts/42/comments'
        });
    });

    // Method handlers ensure the FULL path is matched, so comments.php will not get included twice
    $app->get(function($req) use($app) {
        // Use array notation for variable passing (via Pimple)
        $app['comments'] = array('type' => 'post'); // all comments for 'post' type

        // Include our nested comments
        require 'comments.php'; // will be 'posts/comments'
    });
});
```

This allows the re-use of specific paths and common functionality by nesting
them in multiple contexts with a simple PHP include/require. Bullet is setup
for this out of the box, and even encourages this type of code re-use through
features like relative URL building - another unique feature among PHP
frameworks:

```php
<?php
// RELATIVE url (/posts/25/comments/57, /events/9/comments/57, /comments/57)
echo $app->url('./comments/' . $comment->id);

// ROOT url (always /comments/57)
echo $app->url('/comments/' . $comment->id);
```

### Wrapping Up


This has been a brief overview of the main benefits, but there's a lot more to
get excited about regarding Bullet, and there is [lots of explanation and
documentation up on the Bullet site](http://bulletphp.com) to dig through. It's
a very unique PHP framework that fully embraces and helps your app
automatically conform to the HTTP spec, and I think you'll love using it.


### Get Bullet


View or fork the
[Source on GitHub](https://github.com/vlucas/bulletphp)

Visit the main
[Bullet Website](http://bulletphp.com)

See the
[Composer Package](https://packagist.org/packages/vlucas/bulletphp) on Packagist

Make sure to
[let me know](http://twitter.com/vlucas) if you start using it or if you have any questions or awesome ideas.
