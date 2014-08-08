---
title: Hiding Variables From PHP's <code>var_dump()</code>
date: 2014-08-07 21:09 UTC
tags: ['php', 'programming', 'hacks']
---

A common problem I have in PHP is that when I deal with larger objects with
multiple dependencies or circular references, `var_dump()` becomes effectively
useless. Often times it spits out *huge* numbers of objects that were set as a
dependency on the object I wanted to inspect that I didn't even know were
there. Even worse, there are often circular references and other things that
get in the way. The signal-to-noise ratio is way off the charts.

## Playing Hide-and-seek with `var_dump()`

So is there a way to effectively "hide" a variable from functions like
`var_dump`, `print_r`, and `var_export`? Turns out, there is - but it does come
with some serious trade-offs.

The first thing you need to know is that there is **no way to hide an object
property**. Object properties will all be dumped, no matter the scope - public,
protected, or private.

The trick *(or hack, if you will)* relies on using a very handy but little-used
feature of PHP: static variables. No, not static class properties - static
*variables*. You may have seen them, or even used them yourself a time or two:

```php
<?php
function connection()
{
    static $connection;
    if ($connection === null) {
        $connection = new DatabaseConnection();
    }
    return $connection;
}
```

Basically our `connection` function acts as a kind of factory. The
`$connection` variable here is cached between function calls so that the object
is only ever created once, and then simply returned on subsequent calls. The
`static` declaration before our variable tells PHP that this variable will
maintain its value within the current execution scope, much like a static class
property maintains its value across multiple object creations.

## That's Great, But Now Your Dependecy Is Hard-Coded

The more experienced coders among you may have noticed that using this
approach, we can no longer use constructor dependency injecton. At least - not
*exactly* like we used to.

**Remember when I said solving this problem comes with some serious trade-offs?
Okay, stay with me here. We can still use dependency injection.**

The main design trade-off you will have to make if you want to use this
approach is that you will have to create a single method that is both a getter
and setter for each object dependecy you want to inject. This is best
illustrated by example:

```php
<?php
function connection(DatabaseConnection $newConnection = null)
{
    static $connection;
    if ($newConnection !== null) {
        $connection = $newConnection;
    }
    return $connection;
}
```

Okay, so now we have modified our function to be both a setter and a getter. We
can now use this function to set our dependecy upon object creation, and to get
the connection we need to use in place of the typical object property.

## Using Dependency Injection In An Object

So now, **instead of setting an object property** and using that object property
for our dependency like so:

```php
<?php
class DoStuff
{
    protected $connection;

    public function __construct(DatabaseConnection $connection)
    {
        $this->connection = $connection;
    }

    public function someQuery()
    {
        $this->connection->query("SELECT ... blah blah blah");
    }
}
```

We can **remove the object property and replace it with our new method**, and use
our `connection` method as a setter in the constructor (while still using
dependecy injection), and then use our `connection` method anywhere we need
it as a getter.

```php
<?php
class DoStuff
{
    public function __construct(DatabaseConnection $connection)
    {
        // Setter
        $this->connection($connection);
    }

    public function connection(DatabaseConnection $newConnection = null)
    {
        static $connection;
        if ($newConnection !== null) {
            $connection = $newConnection;
        }
        return $connection;
    }

    public function someQuery()
    {
        // Getter
        $this->connection()->query("SELECT ... blah blah blah");
    }
}
```

And there we have it. We get the same functionality with a little bit more
code, except now our dependant object is **completely hidden from `var_dump`,
because it is not an object property**. It's just a variable inside a method,
which no PHP inspector function will display.

## Benefits To This Approach

1. **The dependency is completely hidden from `var_dump`, `var_export`
   and `print_r`.** This results in nice and clean debugging dumps without any
   large dependency graphs or circular references.
2. **Your database and service credentials are safe**. Related to #1 - this is
   such a huge benefit by itself that is has to have its own point. How many
   times have you dumped the contents of an object only to see a database
   connection dependency or API service dependency also dump out all your
   connection information, usernames, passwords, and API keys? Hopefully this
   never happens in production, but even in development, this is irritating.
3. **Save time and hassle**. Sometimes dumping an object's contents can be
   unexpectedly *gigantic*. In these cases, most of your time spent debugging
   is scrolling through pages and pages of junk you don't need to find the one
   piece of information you're actually looking for. This can be very
   fristrating, especially if you're in an edit-debug cycle where you are
   repeating this multiple times in quick succession.

## Drawbacks To This Approach

So... trade-offs, right? Here are the main ones I have seen:

1. **You have to use a combined getter/setter method**. This is because the static
   variable's scope only exists inside the method it is defined in. I
   personally like single-word combined getter/setter methods, because they
   simplify and reduce the surface area of your object's API, so this may not
   even be a drawback to you.
2. **The object reference will now be static**. This is probably okay for most
   objects you pass that are setup once and remain constant, like a
   `DatabaseConnection` object would be, but it won't be okay for other
   scenarios, like a collection of result objects or anything else that may
   change for each new object instance you want to create.

   *(Update: I previously said the object refence won't change (mutate)
   anymore once set with this method, but my tests on that were flawed.)*
3. **The code might be confusing**. Simplicity has a lot of value.  This
   approach does add a little bit of complexity to the code, and may be a
   little harder to understand and debug, especially for beginners. It's likely
   that people will see this code and say something like *"Why don't you just
   use an object property?"*, because what it is doing is not immediately
   obvious.

If you are using this approach and can think of any other benefits or
drawbacks, [let me know](https://twitter.com/vlucas) and I will update this
post. Hiding variables from `var_dump` and other similar functions is something
I really wish was built-in PHP.

*UPDATE: I have [been
informed](https://twitter.com/cowburn/status/497821547886022657) that [a new
`__debugInfo()` magic method](https://wiki.php.net/rfc/debug-info) has made its
way into PHP 5.6 - this is exactly what I was looking for, so now you just have to
upgrade when PHP 5.6 has a stable release :)*

