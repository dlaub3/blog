---
quality: medium
author:
date: 2018-08-11
draft: false
title: "10 Principles for Clean Code"
description: "10 Principles for learning the art of writing clean code."
tags: ['clean code', 'coding']
categories: ['coding']
toc: true

---

The ultimate goal is code that is easy to read, understand, and maintain. It's code that doesn't require tons of comments because it's so expressive on it's own. 

##   1. Stay DRY

**DRY**: Don't Repeat Yourself

##   2. Be TED

**TED**: Terse, Expressive, Do One Thing

## 3.  Name things carefully

* Watch out for 'And', 'If', and 'Or' in your names, they indicate you're doing multiple things that could be separated.

* Don't abbreviate names, it makes it more confusing later.

* Booleans should sound like true/false questions: 'isAdmin', 'hasActiveAccount', etc.

* Use positive conditionals `if (isNice)` not `if(!isNotNice)`

* Use symmetrical or opposite variables when applicable: 'on/off', 'up/down', etc.

* Good naming should convey intent, that way you don't need a comment to explain what the variable is used for. 

## 4. Use mayfly variables

Mayfly variables are short lived variables that only use memory when they are needed.

## 5. Use intermediate variables. 

```js
var nice = goodAttitute && niceToOthers && stillBelievesInSanta;
if(nice) {
    return getsPresents;
}
```

## 6. Keep things short
There are recommended maximum lengths for all of these.

* Variable Names
* Line Length
* Methods/Functions
* Classes 
* Files 

## 7. Don't use magic numbers

Magic number are numbers that don't mean anything on their own. They make the code harder to maintain.

**Bad**

```js
if (employtee.type == '2'){}
```

**Good**

```js
var manager = 2;
if (employee.type == manager) {}
```

## 8. Return Early

**Bad**

```js
function myFunction(superbig) {
    foreach item in superbig {
       do longrunningtask
    }
    
    if( superbig[0] == false ) {
        return;
    }
    return results
}
```

**Good**

```js
function myFunction(superbig) {
    if( superbig[0] == false ) {
        return;
    }
    
    foreach item in superbig {
       do longrunningtask
    }
    return results
}
```

## 9. Reduce Nested If Else Statements

Ideally, you would never nest if/else statements.

**Bad**

```js
if(x > 20) {
   if(x > 10 && x < 15) {
    	return "almost there"
	}else if(x < 10){
    	if(x < 5) {
    		return "good start"
		}
    } else{
        return "doing better"
    }
} else {
    return "you win"
}
```

**Good**

```js
if(x < 5) {
    return "good start"
}
if(x > 5 && x < 10) {
    return "doing better"
}
if(x > 10 && x < 15) {
    return "almost there"
}
if(x > 20) {
    return "you win"
}
```

## 10. Use Enums

Enums provide a mechanism for giving meaning to arbitrary data. Enums may not be part of the language you work with, but you can at least emulate their behavior. 

```js
var accountLevels = {
    stadard: 300, 
    bronze: 500,
    gold: 800,
    platinum: 1000
};

if(user.accountLevel > accountLevels.standard) {
    enableMoreFeatures;
}
```

## References and Further Learning: 

- [Clean Code Cheat Sheet](https://dokk.org/library/Clean_Code_Cheat_Sheet_v2.4_(Enzler_2014))
- [Uncle Bob's Clean Code](https://www.safaribooksonline.com/live-training/courses/clean-code/0636920194538)
- This post was based largely on Cory's course [Writing Clean Code For Humans](https://www.pluralsight.com/courses/writing-clean-code-humans)

