---
title: 'Get Only Public Class Properties for the Current Class in PHP'
date: 2010-01-05
tags: class-properties, classes, oo, php, php, programming, programming
---

PHP provides two built-in functions to retrieve properties of a given class - 
[get_object_vars](http://php.net/get_object_vars) and 
[get_class_vars](http://php.net/get_class_vars). Both these functions behave the same exact way, one taking an object as a variable and the other taking a string class name. The tricky thing about the two functions is that they behave differently depending on the call scope, returning all of the class variables available within the called scope. So if you call either function within the current class you need properties from, all properties are returned - public, protected, and private - because the current scope has access to them all. This makes seemingly simple things like returning all the public properties within the current class a bit of a pain if you want to keep the code inside the class itself.

more
The obvious solution is to use the functions from a different call scope, which means either moving the call outside the class to the implementation code (yuck), or creating a new function outside the class for it. A lot of suggestions in the PHP manual say to create a new function below the class definition and call it within the class (kind of a proxy function as a workaround), but that seems tacky. Luckily, PHP also provides another way to get a new call scope: 
[anonymous functions](http://php.net/manual/en/functions.anonymous.php) (and 
closures as of PHP 5.3).class BobUser
{
	public $name = 'bob';
	public $publicFlag = true;
	protected $internalFlag = true;
	
	public function getFields()
	{
		$getFields = create_function('$obj', 'return get_object_vars($obj);');
		return $getFields($this); // Returns only 'name' and 'publicFlag'
	}
}

We can also do this the PHP 5.3 way with a much nicer-looking closure:

class BobUser
{
	// ...
	public function getFields()
	{
		$getFields = function($obj) { return get_object_vars($obj); };
		return $getFields($this);
	}
}