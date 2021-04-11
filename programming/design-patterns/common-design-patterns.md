---
date: 2018-08-11T16:36:27-04:00
draft: false
author:
title: "Common Design Patterns"
description: "An overview of common software design patterns"
tags: ["ecmascript", "design patterns"]
categories: ["engineering"]
toc: true
meta:
  description: "An overview of common software design patterns"

---

## Constructor Pattern

The constructor pattern allows you to construct multiple objects of the same kind. Here is an example of a cookie constructor. 

```js
'use strict'

class Cookie {
  constructor({ kind, number } = { kind: "Chocolate Chip", number: 12 }) {
    Object.assign(this, { kind, number });
  };

  bake() {
    console.log(`Baking ${this.number} ${this.kind} cookies.`);
  };

  eat() {
    console.log(`Eating ${this.number} delicious ${this.kind} cookies.`);
  };
}

let chocolateChip = new Cookie();

chocolateChip.bake();
chocolateChip.eat();

let noBake = new Cookie({ kind: "No Bake", number: 24 });

noBake.bake();
noBake.eat();
```

## Factory Pattern 

The factory pattern is a creational design pattern. It allows you to create objects without knowing the implementation details. 

```js
"use strict";

let cookies = ({ kind, number }) => ({
  kind,
  number,
  bake() {
    console.log(`Baking ${this.number} ${this.kind} cookies.`);
  },
  eat() {
    console.log(`Eating ${this.number} delicious ${this.kind} cookies.`);
  }
});

let cookieFactory = cookie => {
  let recipie;

  switch (cookie) {
    case "No Bake":
      recipie = { kind: cookie, number: 12 };
      break;
    case "Butter Scotch":
      recipie = { kind: cookie, number: 24 };
      break;
    default:
      recipie = { kind: "Chocolate Chip", number: 36 };
  }

  return cookies(recipie);
};

let chocolateChip = cookieFactory();

chocolateChip.bake();
chocolateChip.eat();

let noBake = cookieFactory("No Bake");

noBake.bake();
noBake.eat();

```

## Singleton Pattern

The singleton pattern is used to restrict an object to only one instance. It is commonly used with database connections, so the application will use the same connection for all operations.

```js
'use strict'

class CookieJar {
  constructor({ kind, number } = { kind: "Chocolate Chip", number: 12 }) {
    Object.assign(this, { kind, number });
  };

  eatCookies() {
    console.log(`Eating delicious ${this.kind} cookies.`);
  };

  countCookies() {
    console.log(`There are now ${this.number} delicious ${this.kind} cookies.`);
  };
  getCookies(n) {
    this.number = this.number - n;
  }
}

let cookieJar = (CookieJar) => ({
  cookies: null,
  getCookies(n) {
    if (this.cookies === null) {
      this.makeCookies();
    }
    if (this.cookies.number > 0) {
      this.cookies.getCookies(n);
      return this.cookies;
    } else {
      console.log("No more cookies");
    }
  },
  makeCookies() {
    this.cookies = new CookieJar();
  }
});

cookieJar = cookieJar(CookieJar);

let brother = cookieJar.getCookies(10);
let sister = cookieJar.getCookies(2);
sister.eatCookies(); // Eating delicious Chocolate Chip cookies. 
brother.eatCookies(); // Eating delicious Chocolate Chip cookies. 
cookieJar.getCookies(); // "No more cookies"
console.log(sister === brother); // True  ...ok maybe not the best example
```

## Decorator 

A decorator allows you to add functionality to an instance of an object without affecting the original. It can be thought of as a wrapper that provides some desired behavior. Some languages like Python already have built in decorators. But if your language doesn't support decorators natively, you can still build your own. 

```js
'use strict'

class CookieJar {
  constructor({ kind, number } = { kind: "Chocolate Chip", number: 12 }) {
    Object.assign(this, { kind, number });
  };

  eatCookies() {
    console.log(`Eating delicious ${this.kind} cookies.`);
  };

  countCookies() {
    console.log(`There are now ${this.number} delicious ${this.kind} cookies.`);
  };

  getCookies(n) {
    this.number = this.number - n;
  }
}

let cookieJar = (CookieJar) => ({
  cookies: null,
  getCookies(n) {
    if (this.cookies === null) {
      this.makeCookies();
    }
    if (this.cookies.number > 0) {
      this.cookies.getCookies(n);
      return this.cookies;
    } else {
      console.log("No more cookies");
    }
  },
  makeCookies() {
    this.cookies = new CookieJar();
    console.log(`There are now ${this.cookies.number} delicious cookies in the cookie jar.`);
  }
});

let cookieJarDecorator = (cookieJar) => {
  return () => {
    console.log(`Creating a cookieJar`);
    return cookieJar(CookieJar);
  }
};

cookieJar = cookieJarDecorator(cookieJar);
cookieJar = cookieJar(CookieJar);

cookieJar.makeCookies();
```

## Flyweight 

The flyweight pattern is used to keep memory consumption to a minimum when creating a large number of similar objects. 

```js
"use strict";

class Cookie {
  constructor({ kind, number } = { kind: "Chocolate Chip", number: 12 }) {
    Object.assign(this, { kind, number });
  }

  bake() {
    console.log(`Baking ${this.number} ${this.kind} cookies.`);
  }

  eat() {
    console.log(`Eating ${this.number} delicious ${this.kind} cookies.`);
  }
}

let FlyweightFactory = () => {
  let Flyweight = {};

  let makeCookies = recipie => {
    if (typeof Flyweight[recipie.kind + recipie.number] !== "undefined") {
      return Flyweight[recipie.kind + recipie.number];
    } else {
      Flyweight[recipie.kind + recipie.number] = new Cookie(recipie);
      return Flyweight[recipie.kind + recipie.number];
    }
  };

  let countCookies = () => {
    let count = 0;
    for (let i in Flyweight) count++;
    console.log(count);
  };

  return {
    count: countCookies,
    make: makeCookies
  };
};

let cookieFlyweightFactory = FlyweightFactory();
let chocolateChip0 = cookieFlyweightFactory.make({ kind: "Chocolate Chip" });
let chocolateChip1 = cookieFlyweightFactory.make({ kind: "Chocolate Chip" });
let chocolateChip2 = cookieFlyweightFactory.make({ kind: "Chocolate Chip" });
let chocolateChip3 = cookieFlyweightFactory.make({ kind: "Chocolate Chip", number: 13 });
let chocolateChip4 = cookieFlyweightFactory.make({ kind: "Chocolate Chip", number: 23 });

cookieFlyweightFactory.count(); // 3
```

## References

- https://www.pluralsight.com/courses/javascript-practical-design-patterns
- https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841
- https://msdn.microsoft.com/en-us/magazine/hh273390.aspx	
