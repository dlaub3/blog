---
date: 2018-08-11T14:41:27-04:00
draft: false
author:
title: "Common Data Structures"
description: "An overview of common data structures"
tags: ['data structures', 'ecmascript']
categories: ['engineering']
toc: true

---

## Linked List

A linked list is a list data structure where each element in the list contains a pointer to the next element on the list. There is also a doubly linked list in which each element has two pointers, one to the previous element and one to the next element. A doubly linked list can be traversed in two directions. Linked lists can be used in situations where an unknown number of elements need to be added to the list, but you don't want the memory overhead that comes with arrays when they fill and double in size. 

## Stack

A stack is a data structure that is accessed in a LIFO, last in first out, order. It may be implemented with an array.

## Queue

A queue is a data structure that is accessed in a FIFO, first in first out, order. It may be implemented with an array.

## Heap

There are two types of heaps, min heaps and max heaps. Min heaps allow you to access the smallest item first, and max heaps allow you to access the largest item first. They are great because the access to these items is O(1). Heaps may be implemented with arrays. 

## Binary Tree

Trees link data together in a hierarchical way. If you picture a family tree in your mind, you'll have the right idea. There is a single path between nodes and a single top or root node. The side nodes are called leafs. The nodes are thought of in a parent/child relationship. There are several types of binary trees each with their own advantages and disadvantages. 

## Tries

Tries are a type of tree. They are often used for fast word lookup like word completion while your typing. [Tries](https://www.youtube.com/watch?v=zIjfhVPRZCg) 

## Hash

https://www.quora.com/What-are-the-differences-between-Map-Hash-and-Dictionary-in-programming-languages

A hash can also be called an associative array. It is a data structure with key/value pairs. It provides fast lookup of the key values.  Key characteristics of a hash algorithm are: 

- Stable - correct return value
- Uniform - distributed key evenly in available range
- Efficient - balanced cost 
- Secure - difficult to crack

## Graph

A graph is a data structure with multiple paths from one node to another. The paths may be equal in length, or one path may be shorter. It depends on the graph. A graph could be used to model a computer network. 

## References 

- Cracking the Coding Interview by Gayle Laakmann McDowell
- https://www.geeksforgeeks.org/data-structures/
