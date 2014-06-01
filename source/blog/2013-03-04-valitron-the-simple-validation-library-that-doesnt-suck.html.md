---
title: "Valitron: The Simple Validation Library That Doesn't Suck"
date: 2013-03-04
tags: php, programming, validation, validator, valitron
---

[Valitron](https://github.com/vlucas/valitron) is a simple, minimal and elegant stand-alone PHP validation library with NO dependencies. Valitron uses simple, straightforward validation methods with a focus on readable and concise syntax.


### Why Another Validation Library?


[Valitron](https://github.com/vlucas/valitron) was created out of frustration with other validation libraries that have dependencies on large components from other frameworks unrelated to validation like
[Illuminate Validation](https://github.com/illuminate/validation) (laravel 4) requiring 
[Symfony HttpFoundation](http://symfony.com/doc/master/components/http_foundation/index.html), pulling in a ton of extra files that aren't needed for basic validation. It also has purposefully simple syntax used to run all validations in one call instead of individually validating each value by instantiating new classes and validating values one at a time like
[Respect Validation](https://github.com/Respect/Validation) requires you to do. Valitron also has a focus on being concise - validation rules are just a single line per rule, and can include multiple fields in an array. This is handy, because in most use cases, a single validation rule - like "required" will be applied to many fields, so it doesn't make sense to start with the field first like
[Fuel Validation](https://github.com/fuelphp/validation) does.

In short, Valitron is everything you've been looking for in a validation library but haven't been able to find until now: simple pragmatic syntax, lightweight code that makes sense, extensibility for custom callbacks and validations, good test coverage, and no dependencies.


### Usage Example

Valitron is made to setup all your validation rules on the fields you need, and then run all the validations in one call. This is better than validating the fields one-by-one, because that approach causes a lot of "if" statements and branching logic that doesn't make the resulting code any better than doing the validations by hand (which obviously sucks).


```php
<?php
$v = new Valitron\Validator($_POST); // Input array
$v->rule('required', ['name', 'email', 'date_start']);
$v->rule('email', 'email'); // Email uses filter_var
$v->rule('dateAfter', 'date_start', new \DateTime()); // After today
if($v->validate()) {
    echo "Yay! We're all good!";
    print_r($v->data());
} else {
    // Errors
    print_r($v->errors());
}
```

More usage examples and documentation can be found on the
[Valitron GitHub Page](https://github.com/vlucas/valitron). Valitron is
[on Packagist](https://packagist.org/packages/vlucas/valitron), and can be installed via
[Composer](http://getcomposer.org/).

