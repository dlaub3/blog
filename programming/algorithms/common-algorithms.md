---
date: 2018-08-11
draft: false
author:
title: "Common Algorithms"
description: "An overview of common search and sort algorithms"
tags: ['algorithms', 'ecmascript']
categories: ['engineering']
toc: true
fresh: false

---

Algorithms can be broken down into two major categories, searching algorithms and sorting algorithms. 

## Common Sorting Algorithms:

[Visualgo](https://visualgo.net/en/sorting) provides visuals for learning algorithms. 

### Bubble Sort
A bubble sort will pass through an array swapping larger and smaller values until no values are swapped. This may take multiple passes over the array. 

```js
function bubbleSort(array) {
  let l = array.length;
  for(let i = 0; i < l; i++) {
    let swapped = false;
    
    for(let j = 0; j < l - i - 1; j++) {
      if (array[j] > array[j + 1]) {
        [array[j], array[j+1]] = [array[j+1], array[j]] 
        swapped = true;
      }
    }
    if(swapped == false) {
      break;
    } 
  }
  
  return array;
}
```

### Insertion Sort
Insertion sort makes a single pass through the data. All data on the left is considered sorted. 

```js
function insertionSort(array) {
  let l = array.length;
    
  for(let i = 1; i < l; i++) {
    let k = array[i];
    let j = i - 1;
    
    while( j >= 0 && k < array[j]) {
      array[j + 1] = array[j];
      j--;
    }

    array[j + 1] = k;

  }
  return array;
}
```

### Selection Sort
Selection sort, sorts from smallest to largest. It will find the smallest element, move it to the front, and then find the next smallest element etc. 

```js
function selectionSort(array) {
  let l = array.length;

  for(let i = 0; i < l; i++) {
    let min = i;

    for(let j = i + 1; j < l; j++) {
      if(array[min] > array[j]) {
        min = j;
      }
    }

    [array[i], array[min]] = [array[min], array[i]]
  }

  return array;
}
```

### Merge Sort 
Merge sort is great large nearly sorted data sets. A merge sort iteratively haves the data set and then resembles it in sorted order. 

```js
function mergeSort(array) {
  sort(array, 0, array.length -1);
  return array;
}

function merge(a, l, p, r) {
  let sub1 = p - l + 1;
  let sub2 = r - p;
  let i = 0, j = 0, k = l;
  let L = [], R = [];

  for(let i = 0; i < sub1; i++) {
    L[i] = a[l + i];
  }

  for(let j = 0; j < sub2; j++) {
    R[j] = a[p + 1 + j] 
  }

  console.log(L)

  while(i < sub1 && j < sub2) {
    if(L[i]<= R[j]) {
      a[k] = L[i];
      i++;
    } else {
      a[k] = R[j];
      j++;
    }
    k++;
  }

  while( i < sub1 ) {
    a[k] = L[i];
    i++;
    k++;
  }

  while(j < sub2) {
    a[k] = R[j];
    j++;
    k++;
  }
}

function sort(a, l, r) {

  if(l < r) {
    const p = Math.floor((l + r -1)/2);
    sort(a, l, p);
    sort(a, p + 1, r);
    merge(a, l, p, r);
  }
}
```

### Quick Sort
Quick sort is another divide and conquer algorithm. It works similar to merge sort but uses less memory when processing arrays. 

```js
function quickSort(array) {
  let a = array;
  sort(a, 0, a.length - 1);
  console.log(a);
  return a;
}

function sort(a, low, high) {
  if (low < high) {
    let pi = partition(a, low, high);
    sort(a, low, pi -1);
    sort(a, pi + 1, high);
  }
}

function partition(a, low, high) {
    let pivot = a[high];
    let i = low;
    for(let j = low; j < high; j++ ) {
      if(a[j] <= pivot)  {
        [a[j], a[i]] = [a[i], a[j]]
         i++;
      }
    }
    [a[i], a[high]] = [a[high], a[i]]
    return i;
}
```

## Common Searching Algorithms

After data has been sorted, it is much easier to search. It's important to know something about the data you are searching in order pick the best algorithm. 

### Binary Search
A binary search works on sorted data. It will pick the middle element, and see if it is higher or lower than the element is it searching for, based on that it will chose the middle element again from either the right or left of the first choice. This is a fast way to narrow down to the desired item and works well on large data sets. 

There are other types of searching algorithms designed for tree and graphs. These include in order tree traversals and BFS (Breadth First Search). 

## References and Learning

- [Visualgo](https://visualgo.net/en/sorting)
- Cracking the Coding Interview by Gayle Laakmann McDowell
- https://www.geeksforgeeks.org/fundamentals-of-algorithms/
