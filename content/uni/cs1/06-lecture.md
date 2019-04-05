+++
title = "Computer Science I - Lecture 06"
author = ["eo shiru"]
date = 2018-11-16
lastmod = 2019-04-06T00:14:39+02:00
draft = false
+++

Okay so apparently I covered most of what we did in this lecture in the last blog post already (post about lecture 05).

I'll list what we did in this lecture so you can look at those parts in the last blog post (or elsewhere :D ):

-   array initialization (+ with loops)
-   string functions
-   mutlidimensional arrays

Because I didn't really get into multidimensional arrays in the last post and because I don't think the slides are particulary useful on that topic I'll write a bit about them here.

The simplest form of multidimensional array is the two-dimensional array. A two-dimensional array is, in essence, a list of one-dimensional arrays. To declare a two-dimensional integer array of size `[x][y]`, you would write something like `int arrayName[x][y];`.
A two-dimensional array can be considered as a table which will have `x` number of rows and `y` number of columns. A two-dimensional array `a`, which contains three rows and four columns can be shown as follows:

</knowledge-database/images/2D-arr.jpg >

Thus, every element in the array `a` is identified by an element name of the form `a[i][j]`, where `a` is the name of the array, and `i` and `j` are the subscripts that uniquely identify each element in `a`.

Multidimensional arrays may be initialized by specifying bracketed values for each row. Following is an array with 3 rows and each row has 4 columns.

```C
// 3 rows which hold 4 elements each -> 3 * 4 elements = 12
int a[3][4] = {
               {0, 1, 2, 3}, // initialize first row indexed by 0
               {4, 5, 6, 7}, // initialize second row indexed by 1
               {8, 9, 10, 11}, // initialize third row indexed by 2
}
```

The nested braces, which indicate the intended row, are optional. The following initialization is equivalent to the previous example:

```C
int a[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};
```

An element in a two-dimensional array is accessed by using the subscripts, i.e., row index and column index of the array. For example:

```C
int val = a[2][3]; // -> 11
```

The above statement will take the 4th element from the 3rd row of the array which is the last one (11). So you first access the row and then the column. Remember that this is zero indexed :D !

Okay that's it already, so not much new stuff in this lecture ^\_^
