---
title: "Clean Code"
date: 2018-07-29T19:12:53-04:00
draft: true
---



General guidelines for writing clean code. 

**DRY: ** Don't Repeat Yourself



**TED:** 

* Terse 
* Expressive 
* Do One Thing



Return Early: 

**BAD**

```js
function myFunction(x) {
    y = x * 10;
    if( x === 0 ) {
        return 1;
    }
}
```



**Good**

```js
function myFunction(x) {
    if( x === 0 ) {
        return 1;
    }
    y = x * 10;
}
```





Reduce Nested If Else Statements

**Bad**

```js
if( x > 10) {
    if(x > 20) {
        return x / 2;
    } else {
        return x * 2;
    }
    
} else {
    if (x > 5) {
        return x * 2;
    } else {
        return x * 10
    }
}

```



**Good**

```js
if(x > 20) {
    return x / 2;
}
if (x > 10 ) {
    
}
if( x > 10) {
     else {
        return x * 2;
    }
    
} else {
    if (x > 5) {
        return x * 2;
    } else {
        return x * 10
    }
} >
```































