---
title: "PHP Strict Data Types"
date: 2018-07-22
description: "How to use strict data types in PHP"
draft: false
tags: ["PHP"]
categories: ["PHP"]

---






### The problem



PHP doesn't require you to explicitly specify the type. This is referred to as weak typing or loosely typed.  And it's both good and bad.  It can make things easier for the programmer or it can make things hard to debug. 







### Data types

There are three data types in PHP, depending on how you break things down.

**Scalar**: These are single data types like strings, Booleans, integers, and floats. 

**Compound**: Arrays and objects. 

**Special types**: resource, null 





### Type juggling 

PHP uses type juggling to automatically set the type for a variable. Below the variable type is 'juggled' as it's reassigned. 

```php
$clown = 789; //type is int
$clown = 'funny'; //type is string.
$clown = false; //type is boolean
```





### Type casting



If you need a specific data type for an operation you can use type casting. Type casting allows you to coerce data to the type you need for an operation. Type casting is only temporary. The following types are available for type casting. 

```php
(int)
(bool)
(float )
(string)
(array)
(object)
(unset )
```



Here is an example of type casting: 

```php
$typeCast = 123; //int

$typeCast = (string)$typecast;

```



### Argument type declarations

PHP 7 has the most support for argument type declarations. But it is partially supported in PHP 5. In PHP strict typing is optional for scalar types. To set a data type in PHP you use type declarations, also called type hinting. 



You can easily type hint the following in PHP 5.4 and up: 

- Classes 
- Interfaces 
- array
- callable 
- self



This is an example of type hinting.

```php
function dataTypes(DateTime $date, callable $func) {

}
```



**Scalar type declarations**

PHP 7 added support for scalar type declarations. This must be enabled on a file by file basis like this:

```php
<?php 
declare(strict_types=1);
```



Note: Enabling strict mode will affect return type declarations as well. Strict typing applies to function calls made from within the file that has strict typing enabled. So a function call from another file to a function in a file with strict typing enabled will not execute with strict typing unless that original file also has strict typing enabled.  Strict typing is only declared for scalar type declarations. 



Exceptions: Integers may be passed to functions expecting a float. 



The following scalar type declarations are supported in PHP 7.0.

- bool 
- flaot
- int
- string 



Because the function below is using strict typing it will only accept an int. Without strict typing the variable would be type cast to an int and the function would run. 

```php
<?php 
declare(strict_types=1);

$num = '15'; //This will NOT work.

//This function will only accept an integer.
function (int $num) {
  return $num;
}
```





**Return type declarations** 

PHP 7.0 added support for return type declarations. It allows the same types as argument type declarations: 

- Classes 
- Interfaces 
- array
- callable 
- self
- bool 
- flaot
- int
- string 



When strict mode is enabled the return types will not be type cast to the correct type, again type casting is the default behavior. 



This is an example of return type declaration. 

```php
<?php 
declare(strict_types=1);

function strictTypes($var):array {
  return $var; //var must be an array
}
```





**Weak mode vs strong mode**

Weak mode allows type casting while strong mode strictly enforces types.   It's important to know when I strong or weak is enforced. 



If you have file A (strong) and file B (weak), then any reference to return types in file A will execute in strong mode even if it's called from file B. This is because file A is in strong mode. And return type declarations execute in the mode of the file containing them.  However, argument type declarations work differently. Argument type declarations in file A called in file B will execute in weak mode since file B is in weak mode. 



| Containing file mode                     | Calling file mode            | Effect                    |
| ---------------------------------------- | ---------------------------- | ------------------------- |
| argument is declared in a strict mode file | called from weak mode file   | will use type casting     |
| argument is declared in a weak mode file | called from strong mode file | will enforce strict types |
| argument/return type is declared in a strong mode file | called from strong mode file | will enforce all strict   |
| return type is declared in a strong mode file | called from weak mode file   | will enforce strict types |
| return type is declared in a weak mode file | called from strong mode file | will use type casting     |





### References 

[https://www.lynda.com/PHP-tutorials/Strict-Data-Types-PHP/480959-2.html](https://www.lynda.com/PHP-tutorials/Strict-Data-Types-PHP/480959-2.html)

[http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration)

[http://php.net/manual/en/functions.returning-values.php#functions.returning-values.type-declaration](http://php.net/manual/en/functions.returning-values.php#functions.returning-values.type-declaration)

