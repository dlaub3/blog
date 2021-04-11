---
date: 2018-07-28
draft: false
author:
title: "Practical Python Primer"
description: "Basic Python language mechanics"
tags: ['python', "language basics"]
categories: ["python", "language basics"]
toc: true
meta:
  description: "Basic Python language mechanics"
  meta: "learning python"

---

Python was developed by Dutch mathematician Guido van Rossum who  is still the principle author and considered the BDFL (Benevolent Dictator for Life).

## [The Zen of Python](https://www.python.org/dev/peps/pep-0020/#id3)  

* Beautiful is better than ugly.
* Explicit is better than implicit.
* Simple is better than complex.
* Complex is better than complicated.
* Flat is better than nested.
* Sparse is better than dense.
* Readability counts.
* Special cases aren't special enough to break the rules.
* Although practicality beats purity.
* Errors should never pass silently.
* Unless explicitly silenced.
* In the face of ambiguity, refuse the temptation to guess.
* There should be one-- and preferably only one --obvious way to do it.
* Although that way may not be obvious at first unless you're Dutch.
* Now is better than never.
* Although never is often better than *right* now.
* If the implementation is hard to explain, it's a bad idea.
* If the implementation is easy to explain, it may be a good idea.
* Namespaces are one honking great idea -- let's do more of those!

## Documentation 

* Python.org/doc

## Language Features 

* Everything is an object in Python 3
* In Python 3 all types are classes. 
* Python requires a function is defined before it is called. 
* Whitespace matters a lot in python. There are no brackets,  blocks are based on indentation. 
* There are no multi line comments
* Python does support some functional programming concepts like higher order functions, and closures but it is not as heavily influenced by functional programming as other languages. 
* Python uses duck typing 
* Integers and strings are immutable in Python
* The 'None' type has a type of 'None' and a value of 'None'. 

## False 

* Empty Strings
* Number 0
* Type None

## Strings

```python
str = '''
Muliline 
string here. 
single quotes in here
should be escaped.
'''

str.upper()
str.lower()

one = 1
two = 2

# python 3.6 supports f strings
numbers = f'{one}, {two}, 3'
print(numbers)

# string are not mutable so they are optimized into one object which makes. 
x = 'string'
y = 'string'

if x is y: 
    print('true')
    
print(id(x))
print(id(y))
```

## Structured Data

### Lists 

Lists are mutable and zero based.

```python
list = [1,2 ,3,4,5]

list[1]

list[1:5:2] # from 1 to 5, step by 2, result is [2,4]
list.index(4) # find index of '4'

list.pop() # remove from end
list.pop(3) # remove at index
del list[0:2:2] # delete at index
print(', '.join(map(str, list)))
print(len(list))
```

### Tupals

A tupal is not mutable.

```python
x = (1,2,3,4,5) # tupal
x = [1,2,3,4,5] # list
x = [1, 'two', 3, 5]

if isinstance(x, tuple):
    print("tupal")
else:
    print("not tupal")
```

### Dictionary

A dictionary is analogous to a hash.

The values can be any type but the key must be immutable -- strings and numbers.

```python
dictionary = { 'one': 1, 'two': 2, 'three':3 }

# OR 
dictionary = dict( one =  1, two = 2, three = 3)
                  
print(dictionary['one']) # this will cause an exeption if a key doesn't exist so 
# so use .get
print(dictionary.get('five'))  
print('one' in dictionary)
```

### Sets

A set is like a list but doesn't allow duplicate elements. 

```python
set = set("this is a set")
print(sorted(set))
```

A list comprehension is a list created from another list or iterator. 

https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions

```python
def main():
	seq = range(11)
	list = [(x, x**2) for x in seq]
    dict = {x: x*2 for x in seq}
	
    if isinstance(seq, list): print("list")
    if isinstance(seq, set): print("set")
```

### Mixed Structures

It's possible to store anything is a data sructrue because everything is an object and variables store references to objects. 

## Conditionals 

* https://docs.python.org/3/tutorial/controlflow.html?highlight=else

```python
if x < y:
   print('x is less than y')       
elif:
   print()
else:
   print()
        
and 
or 
not

is 
is not

y = true
x = 1 if y else 2 # Ternary if in python
print(x)
```

## Loops 

https://docs.python.org/3/tutorial/controlflow.html?highlight=else

```python
# while 
# for 
# continue, break, else
# else only executes if the loop ends normally
# same controls for 'for' loops

x = 10
while x <= 20:
	x += 1
tupal = (1, 2, 3, 4, 5)
for item in tupal: 
	print(item)


numbers = [1, 2, 3, 4, 5]

for i in numbers: 
	print(i)
    
# range(start, end, step)
print(list(range(0, 100, 5)))

x = list(range(1..25))
for n in range(100): # or for n in range(2, n):
    print(n)

# for with dictionary
x = {'one':1, 'two': 2}
for k,v in x.items():
    print('k: {}. v: {}'.format(k, v))  
```

## Arithmetic Operators 

```python
+ 
- 
* 
% 
/ # py3 returns a float
// #py3 returns int


# unary operators
z = 4
z = -z
z = +z
```

## Bitwise Operator

They operate on individual bits not the text representation.

```python
&
|
^  # Xor
<< # shift left
>> # shift right
~  # bitwise inversion

a = 0
b = 1
print(a|b)
print(a ^ b)
print(a & b)
```

## Comparison Operators

```python
< 
>
<=
>= 
==
!= 

# == does not compaire types only values. 
>>> 1 == '1'
False
>>> 1 == 1.0
True
>>> 1 == True
True
```

## Boolean Operators

```python
and
or
not
in # value in set
not in # value not in set 
is # same object identity
is not # not same obect identity
```

## Operator Precedence 

This is similar to math. 

https://docs.python.org/3/reference/expressions.html

| Operator                                                     | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`lambda`](https://docs.python.org/3/reference/expressions.html#lambda) | Lambda expression                                            |
| [`if`](https://docs.python.org/3/reference/compound_stmts.html#if) – [`else`](https://docs.python.org/3/reference/compound_stmts.html#else) | Conditional expression                                       |
| [`or`](https://docs.python.org/3/reference/expressions.html#or) | Boolean OR                                                   |
| [`and`](https://docs.python.org/3/reference/expressions.html#and) | Boolean AND                                                  |
| [`not`](https://docs.python.org/3/reference/expressions.html#not) `x` | Boolean NOT                                                  |
| [`in`](https://docs.python.org/3/reference/expressions.html#in), [`not in`](https://docs.python.org/3/reference/expressions.html#not-in), [`is`](https://docs.python.org/3/reference/expressions.html#is), [`is not`](https://docs.python.org/3/reference/expressions.html#is-not), `<`, `<=`, `>`, `>=`, `!=`, `==` | Comparisons, including membership tests and identity tests   |
| `|`                                                          | Bitwise OR                                                   |
| `^`                                                          | Bitwise XOR                                                  |
| `&`                                                          | Bitwise AND                                                  |
| `<<`, `>>`                                                   | Shifts                                                       |
| `+`, `-`                                                     | Addition and subtraction                                     |
| `*`, `@`, `/`, `//`, `%`                                     | Multiplication, matrix multiplication, division, floor division, remainder [[5\]](https://docs.python.org/3/reference/expressions.html#id21) |
| `+x`, `-x`, `~x`                                             | Positive, negative, bitwise NOT                              |
| `**`                                                         | Exponentiation [[6\]](https://docs.python.org/3/reference/expressions.html#id22) |
| `await` `x`                                                  | Await expression                                             |
| `x[index]`, `x[index:index]`, `x(arguments...)`, `x.attribute` | Subscription, slicing, call, attribute reference             |
| `(expressions...)`, `[expressions...]`, `{key: value...}`,`{expressions...}` | Binding or tuple display, list display, dictionary display, set display |

## Functions 

Functions are like functions and sub routines in other languages.  By default all functions return a value. If there is no return the keyword 'None' is returned. 

Python doesn't support forward declaration. 

> In computer programming, a **forward declaration** is a **declaration** of an identifier (denoting an entity such as a type, a variable, a constant, or a function) for which the programmer has not yet given a complete definition. 
>
> <cite>[Forward Declaration](https://en.wikipedia.org/wiki/Forward_declaration)</cite>

Default arguments are supported. The arguments with defaults should come last. 

If an argument without a default is not passed it will cause an error.  

**In python when you assign a mutable value you are actually assigning a reference to the mutable value.** 

**Scope**: Block doesn't define scope, functions define scope.

```python
def function(n):
	print(n)
	
function('Hello World!')

def say(x = "Hello", y = "World"):
	print(x, y)
say() 
```

## Argument Lists 

*args and **kwargs 

https://www.geeksforgeeks.org/args-kwargs-python/

```python
def main():
    tupal = ('one', 'two', 'three')
	say(*tupal)
    dict = (one = 1, two = 2, thre = 3 )
    

def say(*args):
    if len(args):
        for item in args:
            print(item)
        else: print("Empty")
            
 def sayMore(**kwargs):
    if len(kwargs):
        for k in kwargs:
            print('{} equals {}'.format(k, kwargs[k]))
    else: print("Empty")
```

## Generators 

A special function that us used to create a series of values.

```python
def generator(*args):
    numargs = len(args)
    start = 0
    step = 1
    
    if numargs < 1:
        raise TypeError(f'expected at least 1 argument, recieved{numargs}')
    elif numargs == 1: 
        stop = args[0]
    elif numargs == 2:
        (start, stop) = args
    elif numargs == 3:
        (start,  stop, step) = args
    else: 
        raise TypeError(f'expected at most 3 arguments, recieved{numargs}')	
   
	# the generator
	i = start
    while i <= stop:
        yield i
        i += step
# yield is like return, only it works inside a generator and allows the function to return multiple values.
```

## Decorators 

A decorator is a form of meta programming. It is a special type of function that returns a wrapper function. 

```python
def outer(maybedec): 
	def inner():
		print("From Inner")
        maybedec()
	return inner

closure = outer()
closure()

def maybedecorator()
	print("maybe decorator")

notdecorator = outer(maybedecorator)
notdecorator()
maybedecorator()

# now maybe decorator has been redefined.
# it is only accessibile from outer.
maybedecorator = outer(maybedecorator)
maybedecorator()


# There is a better  syntax
def outer(maybedec): 
   	def inner():
   		print("From Inner")
   	    maybedec()
   	return inner

closure = outer()
closure()

@outer
def maybedecorator()
   	print("maybe decorator")
maybedecorator()
```

## Classes

https://docs.python.org/3/tutorial/classes.html

The first argument to methods is 'self'

```python
class Runner: 
    speed = 'fast'
    food = 'carbs'
    
    def run(self):
        print(self.speed)
	
    def eat(self):
        print(self.food)

def main(): 
    speedy = Runner()
    speedy.run()
    speedy.eat()
    
if __name__ == '__main__': main()
    
```

**Constructing an object.** 

A constructor uses `__init__` to construct an object and initialize properties. The properties are prefixed with `_` as a convention. You access these properties with accessorts (setters/getters).

Variables defined in a class are similar to static variables. 

```python
class Car:
    x = 10 # class variable
    
    # can also pass kwargs to 
    # stay more organized
	def __init__(self, wheels, speed, cost): # instance variables
	self._wheels = wheels if wheels else 4
	self._speed = speed
	self._cost = cost
	
    def speed(self):
        return self._speed

def main():
   bmw = Car(4, 'fast', "$$$")
   print(bmw.speed)
```

**Exceptions**

https://docs.python.org/3/tutorial/errors.html

```python
def main()
	try: 
		n = int('str')
 	except ValueError:
 		print('value error')
 	else: 
        print(n)
if __name__ == '__main__':main()
```

## String Objects

https://docs.python.org/3/library/stdtypes.html#string-methods

Strings are first class objects.

```Python
print('string {}'.format(42 * 7))

class RevStr(str):
    def __str__(self):
        return self[::-1]

# swapcase
# lower
# capitolize
# lower
# upper
# casefold //removecase distinctions even unicode

print('a few words'.swapcase)

# concatenation 
str1 = 'string one'
newstr = 'i have two' ' separate strings'

str3 = str1 + ' ' + newstr

num = 42
answer = 15

print('the answer is {a}, not {n}'.format(a = answer, n = num))

# positional arguments
print('the answer is {0}, not {1}'.format(answer, num))

x = 10 * 10000
print('{:,} is a big number'.format(x))
format(x).replace(',', '.')


s = "i have a large brown lazy dog."
list = s.split()
s2 = ':'.join(list)
print(s.split())
print(s.split('i'))
```

## Python File I/O

https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files

```python
 # 'w'
 # 'a' append
 # 'r+b' 'r+b' Binary/Text
f = open('file.txt', 'r')
for line in f:
    print(line.rstrip())
    
read = open('file.txt', 'rt')
write = open('file-copy.txt', 'wt')
for line in read:
    #print(line.rstrip(), file=output)
    outfile.writelines(line) # doesn't change line endings for the OS
    print('.', end='', flush=True)
outfile.close()
print('\nfinished')

readBin = open('img.jpg', 'rb')
writeBin = open('img-copy.jpg', 'wb')

while True:
    buf = readBin.read(10240) # memomry use at a time
    if buf: 
        writeBin.write(buf)
        print('.', end='', flush=True)
     else: break
     writeFile.close()
     print('\nfinished')   
```

## Built In Functions

* https://docs.python.org/3/library/functions.html

```python

# numbers
int(num)
float(num)
abs(num)
divmod(num)
complex(num)

# strings
repr(str)
ascii(str)
chr(128406)
ord('😋')

# container functions
tupal = (1, 2, 3, 4, 5)
len(tupal)
reversed(tupal)
list(reversed(tupal))
sum(tupal)
sum()
max(tupal)
min(tupal)
any(tupal) # true if any values are true
all(tupal) # if all are true

tupal1 = (1, 2, 3, 4, 5)
tupal2 = (1, 2, 3, 4, 5)
tupal3 = zip(tupal1, tupal2) # zips/pairs the two together

for a,b in tupal3: print(f'{a} - {b}')
for i, x in enumerate(tupal3): # gives index and value
    print(i, x)

# object and class functions
type(x)
id(x)
isintance(x, int)
```

## Modules 

* https://docs.python.org/3/library
* https://docs.python.org/3/library/platform.html

`__name__`  returns the name of the current module. If the file was running from an import  then `__main__` would be the name of the  module, otherwise `__main__` has a special value that means it is the main file.

`if __name__ == '__main__': main()`  ...when the script is imported and run as a module the code in main does not execute.  But when run directly the code in main does execute. This allows you to import the file and use the functions, but the code in main won't execute. So if you only define functions in the script body and only call them in main you can reuse the module. 

```python
def main(): 
    hello()

def hello():
    print('Hello')
    
if __name__ == '__main__': main()

```

```python
import sys
import os
import random
import datetime


def main():
    v = sys.version_info
    print('Python version {}.{}.{}'.format(*v))
    print(sys.platform)
    print(os.name)
    print(os.getenv("PATH"))
    print(os.getrandom(10).hex())
    print(random.randint(1, 1000))
    print(list(range(25)))
    time = datetime.datetime.now()
	print( time.year, time.month, time.day, time.hour, time.minute, time.second, time.microsecond )
              
if __name__ == '__main__': main()
```

## Database Operations

```python
import sqlite3

db = sqlite3.connect('db-api.db')
```

## Exercises 

**Make a module** 

Write a module to give the time in words.

Design data before you write your code. 

## References

- Bill Weinman [http://python.bw.org/](http://python.bw.org/)
- https://www.lynda.com/Python-tutorials/Python-Essential-Training/614299-2.html

