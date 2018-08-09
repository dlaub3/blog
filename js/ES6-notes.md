---
title: "ES 6 Notes"
date: 2018-07-22
tags: ["ES6"]
description: "An introduction to some of the new features in ES6"
---



### Browser support



To see a comprehensive list of support for ES6 see [https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/) 



Because ES6 isn't fully supported it should be transpiled to ES5. Babel is the most popular transpiler.  For small projects you can link to a Babel transpiler and transpile in the browser. However, this is slow and not recommended for working on large projects or for production. 





### A few new features in ES6



#### const and let 

Const and let allow you to define block scoped variables. They are not scope hoisted as the `var` keyword is. Scope hoisting allows you to assign a variable before it is declared. But you cannot do this with `const` a and `let `.

```javascript
x = 5; // assignment 
console.log(x); //using it
var x; // declairing it

y = 5; // assignment 
console.log(y); //using it
let y; // declairing it

let z = 15;
console.log(z);
if(z > 12) { let z = 5; } // the value of z isn't change globally. 
console.log(z);
```

 As you can see ``` let``` allows you to declare variables that are limited to the statement, block, or expression. 



**Const** creates a read-only reference to a variable but it's not immutable.  

```javascript
const dozen = 12; 
const dozen = 13; // will not work 

if (dozen === 12) {
  let dozen = 13; // this will work because JavaScript is block scoped
  console.log(dozen); 
}
console.log(dozen); // dozen is still 12
```

 



#### Template literals 

These are also called template strings or string literals. 

```javascript
`This is a little string I wrote
 might want to ${read} it in a boat. 
 That's weird man. I know man.`
```

Template literals preserve white space and line breaks. They also allow you to include expressions.



#### Arrow functions

```javascript
let add = (a) => { return (b) => (a + b) };
let add5 = add(5);
console.log((add5(15) === 20) ? 'true': 'false')
```

It does more than provide a cleaner syntax. It also helps the problem of ``` this```.  Arrow functions don't have their own ``` this```.  They inherit ``` this``` from the enclosing scope. 

 



###  References

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

[https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/](https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/)

