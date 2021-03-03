---
title: "Async vs Concurrency"
date: 2018-08-28T06:53:20-04:00
draft: false
description: Comparing JavaScript async with Go's goroutines
tags: ['golang', 'ecmascript', 'async', 'concurrency']
categories: ['async', 'concurrency']

---



I spent some time recently considering how JavaScript's asynchronous execution compares with Go's *goroutines*. I found I needed a clear understanding of six concepts to understand the difference. These all relate to code execution in time. 



* Sequential 
* Synchronous 
* Asynchronous 
* Concurrency
* Parallelism 
* Single Threaded 
* Multi Threaded



### Sequential 

> following in sequence
>
> [Webster's Dictrionary](https://www.merriam-webster.com/dictionary/sequential)

Code is read and executed in sequence, from the top of the file to the bottom. That does not mean that one line of code must finish execution before the next line of code can be read. But it does imply an order of operations. Obviously this code will produce the numbers 1 to 4 in order. 

```js
console.log(1);
console.log(2);
console.log(3);
console.log(4);
```



### Synchronous

> happening, existing, or arising at precisely the same time
>
> [Webster's Dictrionary](https://www.merriam-webster.com/dictionary/synchronous)

Dictionary definitions of synchronous and asynchronous do not help when understanding how these terms are used in programming. I think the most helpful thing to consider is how an HTML page is parsed. When a   `<script src="script.js"></script>` tag is read, the browser will stop parsing the HTML, download the script, and then execute the JS before it continues to parse the rest of the HTML. This is how synchronous code executes. Synchronous code will block execution of the remaining lines until it has itself finished execution. In the example below one `say()` function must finish before the other can begin.

```js
function delay() {
  for (let i = 0; i < 1000000000; i++) {
    let x = i;
  }
}

const say = what => {
  for (let i = 0; i < 5; i++) {
    console.log(`${i}: ${what}`);
    delay();
  }
};

const main = () => {
  say('hello');
  say('world');
};

main();
```



### Asynchronous

> not simultaneous or concurrent in time **:** not [synchronous](https://www.merriam-webster.com/dictionary/synchronous)
>
> [Webster's Dictrionary](https://www.merriam-webster.com/dictionary/asynchronous)

Again, the dictionary definition isn't helpful. To use the HTML page example, when the HTML is being parsed and a ``<script src="script.js" async></script>`` (notice `async`) tag is encountered. The script will <u>not</u> prevent the HTML from being parsed. Instead, the browser will begin downloading the script while simultaneously parsing the HTML. Then, once the download has completed the script will be executed. So `async` refers to the ability continue performing one task, while waiting for another task to finish. That other task is commonly one that the application doesn't mange, like a database request, or downloading a remote resource, but it doesn't necessarily have to be. So asynchronous code is continuing to do work while waiting for something else to happen. Promises,  async/await, and generators can all be used to write asynchronous JavaScript. 

```js
const getUser = async name => {
  const user = await new Promise(resolve => {
    setTimeout(() => resolve({ user: name }), 1000);
  });
  console.log("Resolved :)");
  return user;
};

// make mock async request
const dan = getUser("dan");
// do work
for (let i = 0; i < 5; i++) {
  console.log("Waiting...");
}
```



### Concurrency

> **Concurrent computing** is a form of [computing](https://en.wikipedia.org/wiki/Computing) in which several [computations](https://en.wikipedia.org/wiki/Computation) are executed during overlapping time periods—*concurrently*—instead of *sequentially* (one completing before the next starts).
>
> [Concurrent Computing](https://en.wikipedia.org/wiki/Concurrent_computing)

Concurrent computing doesn't require multiple threads. Concurrency means alternating the execution of code blocks. This means do a little of code block 'a', then a little of code block 'b', etc. until the program finishes. Go handles concurrency with *goroutines*. The go runtime schedules the concurrency. In JavaScript concurrency can be created using `async` or generators. But it's more of a manual process.  Below is an example of concurrent JavaScript code written with generators, and concurrent go code. 

```js
class Say {
  constructor(ms = 100, repeat = 1, fn = () => console.log(this.count)) {
    this.ms = ms;
    this.repeat = repeat;
    this.it = this.gen(fn);
    this.count = 0;
  }
  run() {
    this.it.next();
  }
  *gen(fn) {
    while (this.count < this.repeat) {
      this.count++;
      fn();
      yield this.wait();
      if (this.count === this.repeat) {
        console.log("The end.");
      }
    }
  }
  wait() {
    setTimeout(() => {
      this.it.next();
    }, this.ms);
  }
}

let say1 = new Say(100, 5, () => console.log("hello"));
let say2 = new Say(100, 5, () => console.log("world"));
const main = () => {
  say1.run();
  say2.run();
};

main();
```



The following concurrent code is from a  [A Tour of Go](https://tour.golang.org/concurrency/1) .

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	go say("world")
	say("hello")
}
```



### Parallelism 

> **Parallel computing** is a type of [computation](https://en.wikipedia.org/wiki/Computing) in which many calculations or the execution of [processes](https://en.wikipedia.org/wiki/Process_(computing)) are carried out simultaneously.[[1\]](https://en.wikipedia.org/wiki/Parallel_computing#cite_note-1)
>
> [Parallel Computing](https://en.wikipedia.org/wiki/Parallel_computing)

Multiple physical processors are required to have true parallelism. Typically, parallelism is used to perform multiple similar calculations, while concurrency is used with multiple unrelated calculations. Parallelism fits under the broader umbrella of concurrency. So concurrency is not parallelism, but parallel code is concurrent as well. Below is a modified version of the concurrency example above. Because JavaScript is single threaded it can't run in parallel. We'll there are new innovations to make that a possibility, but that's beyond the scope of this article. 

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

func say(s string, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 0; i < 5; i++ {
		fmt.Println(s)
		time.Sleep(100 * time.Millisecond)
	}
}

func main() {
	runtime.GOMAXPROCS(2)
	var wg sync.WaitGroup
	wg.Add(2)
	go say("world", &wg)
	go say("hello", &wg)
	wg.Wait()
}
```



### Single Threaded and Multi Threaded

This is a very simple breakdown. A CPU can have multiple cores; each core can run multiple processes; each process contains 1 or more threads of execution. So a thread is the lowest level execution context. 

JavaScript is single threaded. So it has only one execution context. When JavaScript is written synchronously, the runtime must finish executing each code block before moving on to the next,  this means long running tasks are blocking execution of the rest of the code. But when JavaScript is written asynchronously the runtime is able to move back and forth between code blocks so that one code block doesn't have to complete before another can begin. This ability can be used to write concurrent code in JavaScript. 

In go, a *goroutine* is an abstraction over threads that allow you to easily write concurrent code, single or multi threaded and when properly configured in parallel. 



##### References 

https://blog.golang.org/concurrency-is-not-parallelism

https://computing.llnl.gov/tutorials/parallel_comp/#Whatis

https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop

https://www.ardanlabs.com/blog/2014/01/concurrency-goroutines-and-gomaxprocs.html
