---
date: 2018-08-11T00:00:01
draft: false
author:
title: "Big O"
description: "A basic overview of Big-O Notation"
tags: ['big O', 'algorithms']
categories: ['engineering']
toc: false
meta:
  description: "A basic overview of Big-O Notation"
  meta: "big o notation"

---

Big O notation is used to  mathematically describe the runtime and memory usage required for an algorithm to process data.

Since Big O is a mathematical expression, it's helpful to brush up on your math skills when studying big O. I recommend [Kahn Academy](https://www.khanacademy.org) for this. 

O(1) - Will always run in the same amount of time regardless of the amount of data it's processing. 

O(N) - The runtime will grow linearly with the size of the data.

O(N^2) - Exponential growth. 

As you can see N is the data, or the number of items the algorithm processes. An algorithm with a runtime of O(N^2) is processing each element exponentially. This is often the case for nested for loops like the example below: 

```python
for level1 in numbers:
  result = 0
  for level2 in numbers:
      result += level1 * level2
      print(result)
```

General principles for calculating Big  O are: 

* Different steps get added
* Drop constants
* Different inputs are assigned to different variables
* Drop non-dominant terms

[Big O Explained](https://www.youtube.com/watch?v=v4cd1O4zkGw)

## Logarithms

A logarithm is the inverse of an exponent. Binary searches have logarithmic run times because adding more elements has little effect on the runtime of the algorithm.  A binary search is represented as O(log N). 

## References and Learning

- https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation
- Cracking the Coding Interview by Gayle Laakmann McDowell
