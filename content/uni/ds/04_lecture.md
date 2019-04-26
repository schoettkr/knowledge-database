+++
title = "Datastructures - Lecture 04"
author = ["eo shiru"]
date = 2019-04-12
lastmod = 2019-04-26T13:51:45+02:00
tags = ["uni", "datastructures"]
draft = false
+++

## Efficient Sorting Algorithms {#efficient-sorting-algorithms}


### 1. Quicksort {#1-dot-quicksort}

For applying the quicksort sorting algorithm we first choose a (random) _pivot element_ (Drehpunkt/Mittelpunkt/Achse). The array gets partitioned into two virtual arrays. Virtual in this case means that the original array and the allocated memory for the original array is used for virtual subarrays. The _first "subarray"_ holds all elements which are _less than or equal to_ the pivot element, the other "subarray" holds all elements which are greater than or equal to the pivot element.<br />
The resulting subarrays are then sorted recursively via the same technique:

-   find pivot element
-   divide out elements into two subarrays

This process gets repeated until there are only arrays with one element (cannot be sorted any more). Now the original array is sorted.

**Pseudocode**:<br />

```python
Quicksort(array)
  if (feldlaenge > 1)
    determine pivot element P
    while (there unsorted elements in the array)
      take out element E from array
      if E <= P
        insert E in subarray1
      if E >= P
        insert E in subarray2
Quicksort(subarray1)
Quicksort(subarray2)
```

The performance of the quicksort algorithm is linked to choosing the right pivot element. Quicksort works the best when every partitioning creates two subarrays of equal size. So we should choose a pivot element where the amount of smaller or equal elements is roughly the same as larger or equal element.

Sidenote from slides: Die ambivalente Zuordnung von Elementen, die gleich dem Pivot-Element sind, in entweder das linke oder das rechte Teilfeld ist beabsichtigt und geschieht mit der Zielstellung, die beiden Teilfelder möglichst gleich groß zu halten

There are different strategies to choosing the optimal pivot element. For now we'll go with the element in the middle of the (sub)array.<br />
After choosing a pivot element the other elements have to be distributed into the subarrays. Because we don't want to create new arrays and don't know the boundaries of the subarrays yet, we swap elements in the array as long as there are elements on the left side of the pivot element wich are larger and elements on the right side of the pivot element which are smaller than the pivot element and then swap these.<br />
We run in (aufeiander zu laufen) from both (sub)array boundaries until we find an element that violates the pivot criterion (leftside & larger - rightside & smaller) and swap these elements. We do this until the boundaries overlap.<br />
The position at which the overlap happens, determines the boundary for both resulting subarrays, as well as the final position of the pivot element.

We'll first define a swap helper function:

```C++
void swap (TYPE& a, TYPE& b) {
  TYPE temp = a;
  a = b;
  b = temp;
}
```

Vorbereitender Schritt: Da wir die endgültige Position des Pivot-Elements im Vorhinein nicht kennen, vertauschen wir vor der Aufteilung der Elemente noch das erste Feldelement mit dem Pivot-Element, müssen diese Vertauschung nach der Aufteilung aber wieder rückgängig machen. Dazu vertauschen wir das Element an der ermittelten Grenzposition zwischen den Teilfeldern mit dem in der ersten Feldkomponente zwischengelagerten Pivot-Element.

```C++
void quicksort(int data[], int first, int last) {
  if ((last-first) > 0) { // false if the array has only 1 element and is therefore already sorted
    int lower = first+1, upper = last;
    swap(data[first], data[(first+last)/2]); // take pivot element
    int pivot = data[first];

    while (lower <= upper) {
      while (data[lower] < pivot && lower < last) {
        lower++; // find an element that is larger than pivot in the left subarray
      }
      while (data[upper] > pivot && upper > first) {
        upper--; // find an element that is smaller than pivot in the right subarray
      }

      if (lower < upper) {
        swap(data[lower++], data[upper--]); // swap both elements
      } else {
        lower++;
      }
    }
    swap(data[upper], data[first]); //  move pivot element back to its final position
    quicksort(data, first, upper-1); // sort left subarray
    quicksort(data, upper+1, last); // sort right subarray
  }
}
```

The performance of the quicksort algorithm is largely affected by the chosen pivot element and the hence resulting partitioning into subarrays. In the best case scenario each partitioning yields subarrays with an roughly equal size. This means an array with n elements becomes two subarrays with round about n/2 elements each and n/4 elements in the next step and so on. Therefore the amount of needed comparisons is equal to \\(n + 2 \frac{n}{2} + 4 \frac{n}{4} + 8 \frac{n}{8} + ... + n \frac{n}{n} = n (\log n + 1)\\). This equals a runtime complexity of O(n log n).<br />
In the worst case scenario the partitioning process reduces the original array size in each step by only one. This is the case when one subarray is empty and the other subarray contains all other elements except the pivot element. The amount of required comparisons then roughly equals \\(n-2 + n-3 + n-4 + ... + 1 = \sum\_{i=1}^{n=2} i = (n-1)(n-2)\frac{1}{2}\\). This results in a runtime complexity of O(n^2). The worst case occurs when we continously choose the largest or smallest element as a pivot element. When the array is already sorted and we choose the first element as a pivot element then this would be fatal because the complexity is O(n^2) eventhough the array is already sorted. To avoid this problem with arrays that may already be sorted, we can always choose the middle element (median) as the pivot element. The real median of unsorted elements can of course be computed. However this involves additional steps, that's why we can alternatively draw a sample of 3 elements eg the first, the middle and the last element and use the median of these 3 elements as our pivot element. Of course there are more methods of stabilizing the quicksort algorithm. In practice quicksort is usually the fastest sorting algorithm in most cases (except for small arrays with up to 10 elements - in that case insertionsort might be a better fit).


### 2. Mergesort {#2-dot-mergesort}

The basic idea behind the mergesort algorithm is merging two sorted halfes into one array. Since the halfes need to be sorted themselves, recursion is used until the subarrays/halves arrive at an array length of 1 where they're implicitly in a sorted state. Going up the recursion path these subarrays are then merged and sorted into bigger arrays.
**Pseudocode**:<br />

```python
mergesort(data[])
  if (data[] contains at least two elements)
     mergesort(left subarr of data[])
     mergesort(right subarr of data[])
     merge(data[])
```

To work with only one array, we pass boundaries as parameters. Nevertheless to _merge_ arrays we need a temporary array, with the dimension of the "virtual array" (specified via the passed parameters).

```python
merge(data[], first, last)
  middle = (first + last) / 2;
  pos = 0; left = first; right = middle + 1;
  while (there still elements in either subarr)
    if data[left] < data[right] # element in left subarr is smaller than element in right subarr
      temp[pos++] = data[left++]; # insert element of left subarr into temp[] at pos
    else
      temp[pos++] = data[right++]; # insert element of right subarr into temp[] at pos
  copy temp into data
```

**Performance:**<br />
_1. Data Movements:_<br />
In each call to `merge()` we copy all n elements of the respective array into the temporary array and back. So the amount of data movement operation is 2n. Since mergesort is defined recursively we can determine the count of all data move operations M(n) as the sum of of the movement operations of the two subarrays &rarr; M(n) = 2M(n/2) + 2n. The recursion stops at arrays with one element and there are no operations performed &rarr; M(1) = 0.<br />
By inserting each recursion step and accounting for the halving of the array in each steps, the count of data movement operations can be calculated explicitly:
\\[M(n) = 2M(\frac{n}{2}) + 2n = 4M(\frac{n}{4}) + 4 n = 8M(\frac{n}{8}) + 6n = ... = 2^i M (\frac{n}{2^i}) + 2in = 2n \log n \\]

_2. Comparisons:_<br />
In each execution of `merge()` we compare the n elements of the current array and therefore perform n-1 comparisons. Analogously to the data movements the amount of comparisons can be explicitly calculated as
\\[C(n) = 2^i C(\frac{n}{2^i}) + in - 2^i -1 = n \log n - n + 1\\]

Mergesort has a runtime complexity of O(n log n).


### Quicksort vs Mergesort {#quicksort-vs-mergesort}

Since mergesort has a time complexity of O(n log n) it is in the same complexity category as quicksort. Furthermore mergesort is suitable for external sorting. However merge sort requires additional memory to allow the merging of the sorted subarrays. Because of the additional data movement operations merge sort is in practice generally slower than quicksort.
