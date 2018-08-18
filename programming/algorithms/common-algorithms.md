---
title: "Common Algorithms"
date: 2018-08-11
draft: false
description: "An overview of common search and sort algorithms"
---



Algorithms can be broken down into two major categories, searching algorithms and sorting algorithms. 



##### Common Sorting Algorithms:

[Visualgo](https://visualgo.net/en/sorting) provides visuals for learning algorithms. 

**Bubble Sort**: A bubble sort will pass through an array swapping larger and smaller values until no values are swapped. This may take multiple passes over the array. 



**Insertion Sort**:  Insertion sort makes a single pass through the data. All data on the left is considered sorted. 



**Selection Sort**:  Selection sort, sorts from smallest to largest. It will find the smallest element, move it to the front, and then find the next smallest element etc. 

 

**Merge Sort**: Merge sort is great large nearly sorted data sets. A merge sort iteratively haves the data set and then resembles it in sorted order. 



**Quick Sort**: Quick sort is another divide and conquer algorithm. It works similar to merge sort but uses less memory when processing arrays. 



##### Common Searching Algorithms

After data has been sorted, it is much easier to search. It's important to know something about the data you are searching in order pick the best algorithm. 



**Binary Search**: A binary search works on sorted data. It will pick the middle element, and see if it is higher or lower than the element is it searching for, based on that it will chose the middle element again from either the right or left of the first choice. This is a fast way to narrow down to the desired item and works well on large data sets. 



There are other types of searching algorithms designed for tree and graphs. These include in order tree traversals and BFS (Breadth First Search). 



##### References and Learning

[Visualgo](https://visualgo.net/en/sorting)

Cracking the Coding Interview by Gayle Laakmann McDowell

https://www.geeksforgeeks.org/fundamentals-of-algorithms/