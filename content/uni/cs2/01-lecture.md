+++
title = "Computer Science II - Lecture 01"
author = ["eo shiru"]
date = 2019-04-04
lastmod = 2019-04-07T14:21:08+02:00
tags = ["uni", "cs2"]
draft = false
+++

Computer Science II basically continues where CS I left off. Both lectures are based on a script that is split into two parts. These weeks lecture introduced pointers. Therefore I'll keep it short because there already a lot of posts for lectures regarding pointers on this page :) and at this point is not really a new or special topic.


## Pointers {#pointers}

Every variable has a memory adress. In case of arrays the base adress of the variable points to the beginning of an array eg the `&a` is the same adress as `&a[0]`. Since arrays are stored consecutively in memory this is how the third (index = 2) element, or rather it adress, of an int array `integers` would be retrieved: `integers[0] + 2 * 4`, more generally that equals to `base_adress + index * (size of the underlying data type)`. See for yourself:

```C++
int integers[8] = {1,2,3,4,5,6,7,8};

cout << &integers << endl;
cout << &(integers[0]) << endl;
cout << &integers[1] << endl;
```

```text
0x7ffe0c455460
0x7ffe0c455460
0x7ffe0c455464
```

As you can see the adress of the second element equals the adress of the first element + 4.
Also when you increment a pointer eg `ptr++` it is _not_ incremented by **1** but instead incremented by the size of (in bytes\*) the data type of the value that the pointer currently points to.<br />
So it is totally possible to compute the some of an integer array like this:

```C++
int b[4];
int sum() {
  int* ipointer;
  int sum = 0;
  // for (ipointer = &b[0]; ipointer < &b[4]; ipointer++)
  // alternatively:
  for (ipointer = b; ipointer < b+4; ipointer++)
    sum += *ipointer;
  return sum;
}
```

Don't do this though, this is just for demonstration purposes :D. Note that while doing arithmetics with pointers and integers is valid, you cannot do crazy stuff like `ptr1 + ptr2` which is not a valid operation for the `+` operator and two pointer operands. Comparing pointers via `==` is totally valid though.<br />
It is also possible to _dynamically_ allocate & delete memory for pointers (goes on the heap, which has to be managed by the programmer), like this:

```C++
myPointer = new int;
delete myPointer; //freed memory
myPointer = NULL; //pointed dangling ptr to NULL; same as 0
```

More on heap & stack [here](https://www.gribblelab.org/CBootCamp/7%5FMemory%5FStack%5Fvs%5FHeap.html).<br />
Pointers prove to be especially usefull when we want to for example "pass" an array to a function, want to "return" multiple things from a function (achieved by passing pointers and manipulating the values stored there), work with "large" data object or dynamic data structures.
Here's an example to find the min and max value in an array via one function:

```text
1 | 4
```

It is also possible to use qualifiers like `const` or `volatile` when declaring pointers:

```C++
int i,j;
int* const p = &i;
p = &j; // Error
*p = 11; // Allowed
```

\*technically the size is measured in the number of char-sized storage units which is usually 8 bits and therefore 1 byte - but this is not guaruanteed; what is guaranteed is that `sizeof(char)` is always equal to 1 (more [here](https://en.wikipedia.org/wiki/Sizeof))
