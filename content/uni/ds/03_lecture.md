+++
title = "Datastructures - Lecture 03"
author = ["eo shiru"]
date = 2019-04-11
lastmod = 2019-04-25T12:17:32+02:00
tags = ["uni", "datastructures"]
draft = false
+++

## Simple Sorting Algorithms {#simple-sorting-algorithms}

In this lecture we're looking at simple array sorting algorithms. There are multiple criteria for judging an (sorting) algorithm:

-   performance
    -   amount of comparison operations
    -   amount of data shifts (Datenverschiebungen)
    -   we neglect unimportant details and only count comparison and shifts of the data that we want to sort
-   data distribution
    -   some of the algorithms differ in their efficiency in regards to how the data is sorted beforehand
    -   we distinguish between data that is already sorted (best case), data that is inversly sorted (worst case) and data that is unsorted (average case)
-   decomposability (Zerlegbarkeit)
    -   is the sorting algorithm suitable for external sorting?
    -   the data set might be too large to fit into memory &rarr; sorting algorithm needs to be able to split the data, sort parts of it and then piece the whole sorted data set together
    -   so to be precise, data needs to be read from secondary storage (SSD, HDD) and written to secondary storage after it has been sorted
        -   since this is a lot slower/intensive than accessing primary storage, these operations should be minimized
-   data set
    -   size of the sorting criteria
        -   are the compared elements just simple integers then comparison operations can be executed comparably fast - however when the data set consists of more complex elements such as strings, vectors or compound data types then the required time to perform comparisons increases a lot
    -   dataset size
        -   with an increasing data set size, the importance of shifting/moving elements increases as well
-   complexity
    -   the complexity of the underlying procedures matters in the decision regaring sorting algorithms
    -   sorting scenario 1: once in a while a few elements are sorted
        -   when self-implementing an easy 8 liner may be better than a more efficient but really complex and therefore errorprone sorting algorithm implementation
        -   make use of library functions
    -   sorting scenario 2: frequently a lot of elements are sorted
        -   when self-implementing the speed difference between a seemingly neglectable 10% faster algorithm should not be ignored
        -   library functions need be inspected in regards to their underlying sorting technique


### 1. Bubble Sort {#1-dot-bubble-sort}

As most of the sorting algorithms require some functionality to swap elements in an array we prepare a function that does exactly this via call by reference:

```C++
void swap (TYPE& a, TYPE& b) {
  TYPE temp = a;
  a = b;
  b = temp;
}
```

Each call of `swap` performs 3 data operations.<br />
The bubble sort algorithms works by repeatedly swapping the adjacent elements if they are in wrong order until no more swaps have been performed iterating over the whole array.

```C++
void bubbleSort (TYPE data[], int n) {
  bool swapped = true;
  while (swapped) {
    swapped = false;
    for (int i = 1; i < n; i++) {
      if (data[i-1] > data[i]) {
        swapped = true;
        swap(data[i-1], data[i]);
      }
    }
  }
}
```

In the best case scenario there are no swap operations and n-1 comparisons. In the worst case scenario we have as many swap operations as comparisons. In the average case we have \\(\frac{n(n-1)}{4}\\) swaps.
