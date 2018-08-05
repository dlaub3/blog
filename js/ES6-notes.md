---
title: "ES 6 Notes"
date: 2018-07-22
tags: ["ES6"]
---


### Browser support



To see a comprehensive list of support for ES6 see [https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/) 



Because ES6 isn't fully supported it should be transpiled to ES5. Babel is the most popular transpiler.  For small projects you can link to a Babel transpiler and transpile in the browser. However, this is slow and not recommended for working on large projects or for production. 





### A few new features in ES6



#### let

Let is used to define variables. Here is an example of it's usage. 

```javascript
var x = 10; //global variable 

if(y > 12) { let x = 5; } //the value of x isn't change globally. 
```

 As you can see ``` let``` allows you to declare variables that are limited to the statement, block, or expression. 



#### const 

Const creates a read-only reference to a variable but it's not immutable. 

```javascript
const dozen = 12; 
var dozen = 13; //will not work 

if (dozen === 12) {
  let dozen = 13; //this will work
  
  //dozen is now 13
  console.log(dozen); 
}

//dozen is still 12.
console.log(dozen);
	
```

 



#### Template literals 

These are also called template strings or string literals. 

Template literals are used for strings. 

```javascript
`This is a little string I wrote
 might want to ${read} it in a boat. 
 That's weird man. I know man.`
```

Template literals preserve white space and line breaks. They also allow you to include expressions. 



#### Arrow functions

While the new syntax of arrow functions is quite nice...

```javascript
var x = () => args;

var num = [
  1,
  2,
  3,
  4,
  5
]

var numf = num.filter(num => (num > 3));

```

It does more than provide a cleaner syntax. It also helps the problem of ``` this```.  Arrow functions don't have their own ``` this```.  They inherit ``` this``` from the enclosing scope. 





### ES7

Interested in ES7? Check out [https://github.com/tc39](https://github.com/tc39)

 

###  References

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

[https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/](https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/)

