---
title: "Observer Design Pattern"
date: 2018-09-03
draft: false
description: Implementing the observer design pattern.
categories: ["engineering"]
tags: ["ecmascript"]
toc: false

---

The observer pattern notifies a list of subscribers when changes are made to the observed data.

```js
class Observer {
  constructor() {
    this.observers = [];
  }

  add(fn) {
    if (fn && !this.observers.includes(fn)) {
      this.observers.push(fn);
    }
  }

  remove(fn) {
    this.observers = this.observers.filter(obs => obs !== fn);
  }

  notify(key, val) {
    this.observers.forEach(fn => fn(key, val));
  }
}

const birds = {
  one: 'Cardinal',
  two: 'Blue Jay',
  three: 'Blue Bird',
};

const obs = new Observer();

const handler = {
  get(obj, key) {
    return obj[key];
  },
  set(obj, key, val) {
    obj[key] = val;
    obs.notify(key, val);
    return true;
  },
}

const birdsproxy = new Proxy(birds, handler);

const obs1 = (key, val) => console.log(`Observed subject: ${key} => ${val}`);
obs.add(obs1);
birdsproxy.one = "Blue Bird";
birdsproxy.four = "Black Bird";
obs.remove(obs1);
birdsproxy.four = "Robin";
```

## References 

- https://www.dofactory.com/javascript/observer-design-pattern
- https://hackernoon.com/observer-vs-pub-sub-pattern-50d3b27f838c
- https://otaqui.com/blog/1374/event-emitter-pub-sub-or-deferred-promises-which-should-you-choose/index.html
