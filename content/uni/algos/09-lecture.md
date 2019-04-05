+++
title = "Algos & Programming - Lecture 09"
author = ["eo shiru"]
date = 2018-11-05
lastmod = 2019-04-06T00:17:34+02:00
draft = false
+++

## Complex Types in Python {#complex-types-in-python}

There are built-in types in Python which offer similar functionality to the complex types in C and more.


### Python Strings {#python-strings}

Strings are character sequences of a fixed length and are to be precise **not** a complex type (why?).
String literals are enclosed by double or single quotes (and triple quotes to span eg multiple lines; single quotes are most used).

Python strings are "immutable" which means they cannot be changed after they are created. Since strings can't be changed, we construct **new** strings as we go to represent computed values. So for example the expression `'hello' + 'there'` takes in the 2 strings `'hello'` and `'there'` and builds a new string `'hellothere'`. The resulting string has a different `id()` because a new object is created and the others may then be garbage collected.

Python string also recognize escape sequences similarly to C for example `'\n'`  is recognized as a ASCII Linefeed. To prevent parsing of those one may use a _raw string_ by prepending and `r` or `R` to the string:

```python
print("H\ni")
print(r"H\ni")

a = "Hello"
print("Id of " + a + " " + str((id(a))))
a += " World"
print("Id of " + a + " " + str((id(a))))
```

```text
H
i
H\ni
Id of Hello 139807144629000
Id of Hello World 139807144032560
```

Individual characters of a string can be accessed via `[]` bracket notation.


### Python Lists {#python-lists}

Python has a great built-in list type named "list". List literals are written within square brackets `[ ]`. Lists work similarly to strings - use the `len()` function and square brackets [ ] to access data, with the first element at index `0`. To access elements from the back use negative indices.

```python
colors = ['red', 'blue', 'green']
print(id(colors))
print(id(colors[0]))

test = 'red'
print(id(test))


print(id(colors[1]))
print(id('blue'))
```

Assignment with an `=` on lists does not make a copy. Instead, assignment makes the two variables point to the one list in memory.

The "empty list" is just an empty pair of brackets `[ ]`. The `+` works to append two lists, so `[1, 2] + [3, 4]` yields `[1, 2, 3, 4]` (this is just like `+` with strings).

Lists in Python can (in contrast to arrays in C) hold values of different types.

Python lists are also _mutable_. In fact there are a lot of _methods_ to modify lists eg `append`, `insert`, `remove`, `pop`.


### Python Tuples {#python-tuples}

Tuples in Python are similar to lists with the difference that they are **immutable**.

A tuple is a sequence of immutable Python objects. Tuples are sequences, just like lists. The differences between tuples and lists are, the tuples cannot be changed unlike lists and tuples use parentheses, whereas lists use square brackets.

Creating a tuple is as simple as putting different comma-separated values. Optionally you can put these comma-separated values between parentheses also. For example:

```python
tup1 = ('physics', 'chemistry', 1997, 2000)
tup2 = (1, 2, 3, 4, 5 )
tup3 = "a", "b", "c", "d"
```

The empty tuple is written as two parens containing nothing `tup1 = ()`. To write a tuple containing a single value you have to include a comma, even though there is only one value `(77,)`

Like string indices, tuple indices start at 0, and they can be sliced, concatenated, and so on.

However if tuples are eg concatenated via `+` a new tuple is created:

```python
T = (1,2)
print("tuple: ", T, "with id: ", id(T))
T += (3,4)
print("tuple: ", T, "with id: ", id(T))
```

```text
tuple:  (1, 2) with id:  140023051332616
tuple:  (1, 2, 3, 4) with id:  140023051304856
```


### Python Sets {#python-sets}

Since Python 2.6 there are set types (Mengentypen).
A Set in Python is a data structure equivalent to sets in mathematics. It may consist of various elements; the order of elements in a set is undefined. You can add and delete elements of a set, you can iterate the elements of the set, you can perform standard operations on sets (union, intersection, difference). Besides that, you can check if an element belongs to a set.

Unlike arrays, where the elements are stored as ordered list, the order of elements in a set is undefined (moreover, the set elements are usually not stored in order of appearance in the set; this allows checking if an element belongs to a set faster than just going through all the elements of the set).

Any immutable data type can be an element of a set: a number, a string, a tuple. Mutable (changeable) data types cannot be elements of the set. In particular, lists cannot be an element of a set (but tuple can), and another set cannot be an element of a set. The requirement of immutability follows from the way how do computers represent sets in memory.

Sets unlike lists or tuples can't have multiple occurrences of the same element &rarr; `set('a','b','c','a','b','c')` &rarr; `{'a','b','c'}` no values are duplicated.

To create a set the `set()` is called which constructs a Python set from the given iterable and returns it.

```python
# empty set
print(set())

# from string
print(set('Python'))

# from tuple
print(set(('a', 'e', 'i', 'o', 'u')))

# from list
print(set(['a', 'e', 'i', 'o', 'u']))

# from range
print(set(range(5)))
```

```text
set()
{'y', 't', 'n', 'P', 'o', 'h'}
{'e', 'a', 'u', 'i', 'o'}
{'e', 'a', 'u', 'i', 'o'}
{0, 1, 2, 3, 4}
```

Sets are implemented in a way, which doesn't allow mutable objects. The following example demonstrates that we cannot include, for example, lists as elements:

```python
cities = set((("Python","Perl"), ("Paris", "Berlin", "London"))) # valid set of tuples

cities = set((["Python","Perl"], ["Paris", "Berlin", "London"])) # -> TypeError: unhashable type: 'list'
```

Although sets can't contain mutable objects, sets are mutable themselves. Elements may for example added via the `add` method (`cities.add("Tokyo")`).

Frozensets are like sets except that they cannot be changed so they are immutable:

```python
cities = frozenset(["Frankfurt", "Basel","Freiburg"])
cities.add("Strasbourg") # AttributeError: 'frozenset' object  has no attribute 'add'
```

There's also a simplified shorter notation to construct sets:

```python
cities = {"London", "Paris", "Madrid"}
print(type(cities))
```

```text
<class 'set'>
```

The known operations from set theory are also available via Python Sets (following part in German xD):

-   `len(s)` gibt Mächtigkeit der Menge `s`
-   `s1 | s2` gibt Vereinigungsmenge von `s1` und `s1`
-   `s1 & s2` gibt Schnittmenge von `s1` und `s1`
-   `s1 - s2` gibt Differenzmenge von `s1` und `s1`
-   `s1 ^ s2` gibt symmetrische Differenzmenge von `s1` und `s1`


### Python Dictionaries {#python-dictionaries}

A dictionary is a collection which is unordered, changeable (mutable) and indexed. In Python dictionaries are written with curly brackets, and they have keys and values.

```python
thisdict =	{
  "key": "value",
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}
print(thisdict)
```

```text
{'key': 'value', 'brand': 'Ford', 'model': 'Mustang', 'year': 1964}
```

You can access the items of a dictionary by referring to its key name, inside square brackets:

```python
thisdict =	{
  "key": "value",
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}
print(thisdict["key"])
print(thisdict["model"])
print(thisdict["year"])
```

```text
value
Mustang
1964
```

More on dictionaries and how to work with them / their methods [here](https://www.w3schools.com/python/python%5Fdictionaries.asp)


## Loops and Iterations {#loops-and-iterations}

In C there are three types of loops:

-   while loop
-   do while loop
-   for loop

All of these loops use conditions are able to simulate one another. Which one to use is often a matter of personal taste.


### While Loop {#while-loop}

The while loop executes as long as a conditions is true resp. until a condition is false. This is the procedure:

1.  Check if condition `while (expression/Condition)` evaluates to true (nonzero)
2.  If yes: execute body of the loop and jump to 1.
3.  If no: resume program execution after the loop

```C
// Program to find factorial of a number
// For a positive integer n, factorial = 1*2*3...n

#include <stdio.h>
int main()
{
    int number;
    long long factorial;

    // printf("Enter an integer: ");
    // scanf("%d",&number); // cannot read from stdio in my blog :)
    number = 5;

    factorial = 1;

    // loop terminates when number is less than or equal to 0
    while (number > 0)
    {
        factorial *= number;  // factorial = factorial*number;
        number--;

        // alternatively: factorial *= number--;
    }

    printf("Factorial= %lld", factorial);

    return 0;
}
```

```text
Factorial= 120
```

Loops can be used to fill fields of an array:

```C
#include <stdio.h>
enum { arraySize = 12 }; // constant for array size

int main()
{
  int arr[arraySize], i = 0;

  while (i < arraySize) { // 0,1,2...11
    arr[i] = i*i;
    i++;
  }

  i = 0;
  while (i < arraySize) { // 0,1,2...11
    printf("Element %d of arr: %d\n", i, arr[i]);
    i++;
  }

  return 0;
}
```

```text
Element 0 of arr: 0
Element 1 of arr: 1
Element 2 of arr: 4
Element 3 of arr: 9
Element 4 of arr: 16
Element 5 of arr: 25
Element 6 of arr: 36
Element 7 of arr: 49
Element 8 of arr: 64
Element 9 of arr: 81
Element 10 of arr: 100
Element 11 of arr: 121
```


### Do While Loop {#do-while-loop}

The do while loop is similar to the while loop with the difference being that the do while loop checks the condition **after** it has run, therefore it always runs at least one time.
The syntax is

```C
do {
  statement(s);
} while (expression/condition); // notice the semicolon!
```


### For Loop {#for-loop}

A for loop is a repetition control structure that allows you to efficiently write a loop that needs to execute a specific number of times.

```C
for (init; condition; mutation(eg increment/decrement)) { // "conditon" and "mutation" are expressions (see C standard) but that is how they're used commonly
  statement(s);
}
```

Here is the flow of control in a 'for' loop :

-   the init step is executed first, and only once
    -   this step allows you to declare and initialize any loop control variables
    -   you are not required to put a statement here, as long as a semicolon appears
-   next, the condition is evaluated
    -   if it is true, the body of the loop is executed
    -   if it is false, the body of the loop does not execute and the flow of control jumps to the next statement just after the 'for' loop
-   after the body of the 'for' loop executes, the flow of control jumps back up to the mutation statement
    -   this statement allows you to update any loop control variables
    -   This statement can be left blank, as long as a semicolon appears after the condition
-   the condition is now evaluated again
    -   if it is true, the loop executes and the process repeats itself (body of loop, then mutation step, and then again condition)
    -   after the condition becomes false, the 'for' loop terminates

Omitting expressions:

```C
#include <stdio.h>

int main(int argc, char* argv[])
{
  int i = 0;
  for (; i < argc; i++) // omitting initalization
    printf("%d. argument: %s\n", i+1, argv[i]);


  for (;;) // endless loop
    printf("The answer is 42\n");


  return 0; // never reached
}

```

Dont forget it is possible to have more complex conditions and multiple assignments!

```C
#include <stdio.h>

int main()
{
  for (int i=2, j=1; i<3 || j<5; i++,j++)
    {
      printf("%d, %d\n",i ,j);
    }
  return 0;
}
```

```text
2, 1
3, 2
4, 3
5, 4
```


### `break` and `continue` {#break-and-continue}

Loops run as long as the loop condition evaluates to true. There are however two ways to modify the control flow from _inside the function body_:

-   `break` stops execution of the loop regardless of the loop condition
-   `continue` immediately starts the next evaluation of first the loop condition and possibly the next loop iteration

`break` and `continue` should be used sparsely - an excessive use might be an indicator for insufficient program logic.

```cpp
/* reciprocal .c -- calculate reciprocal value of array elements */
#include <stdio.h>

/* a negative value indicates end of list */
double f[]= {1.0 , .5 , 3.1415 , .33333 , 0.0 , 2.7182 , 42.23 , -1};

int main ()
{
  int i ;
  for (i = 0; ; i++) {
    if (f[i] < 0.0) break ;
    if (f[i] <= 0.0001) continue ;
    f[i]= 1/f[i];
  }

  for (i = 0; f[i] >= 0.0; i++) // notice the condition!
    printf ("%d value : %f\n", i, f[i]);

  return 0;
}
```

```text
0 value : 1.000000
1 value : 2.000000
2 value : 0.318319
3 value : 3.000030
4 value : 0.000000
5 value : 0.367891
6 value : 0.023680
```


## Loops in Python {#loops-in-python}

There are two types of loops in Python `for` and `while`.
The `while` loops are similar to their pendants in C:

```python
i = 5;
while (i > 0):
    print("i =", i)
    i = i -1
```

```text
i = 5
i = 4
i = 3
i = 2
i = 1
```

`break` and `continue` are also available in Python with the same semantic as in C.

In addition to what we know from C in Python there can be an optional `else` branch after a loop which is executed **after the loop finished** if it was **not canceled**:

```python
i = 0;

while (i < 3):
    print(i)
    i += 1;
else:
    print("1. Loop finished execution")

print("------------")
i = 0;
while (True):
    print(i)
    i += 1;
    if (i == 3):
        break
else:
    print("2. Loop finished execution") # not executed because loop doesnt finish naturally but is canceled
```

```text
0
1
2
1. Loop finished execution
------------
0
1
2
```

`else` can also be used with the for loop in Python!

The `for` loop in Python iterates over a given _sequence_ or _set_ (an iterable type!):

```python
for i in ITERABLE:
    do_something(i)
```

In the example above the elements of `ITERABLE` are assigned to `i` one by one per iteration.

```python
for x in ['John','Paul','George','Pete']:
 print (x)
```

```text
John
Paul
George
Pete
```

This also works for mixed types:

```python
T = ('Alpha', 48, 0.2, (1, 2, 3))
for x in T:
    print(2 * x)
```

```text
AlphaAlpha
96
0.4
(1, 2, 3, 1, 2, 3)
```

To "mimic" the C for loop we can iterate over a _sequence_ of numbers using the `range` function:

```python
# range([start,] stop [, step]) # generate from 'start' up to and NOT INCLUDING 'stop' and increment by 'step'

for i in range(0, 6, 2):
    print(i)

print("------------")

for i in range(3):
    print(i)
```

```text
0
2
4
------------
0
1
2
```


## Iterators and Loop Functions in C {#iterators-and-loop-functions-in-c}

Code sample of an iterator with C:

```C
#include <stdio.h>

char *names[] = {"John", "Paul", "George", "Ringo", NULL};

static int index = 0;

char* first() {
  return names[index = 0]; // so the iterator can be used multiple times -> first ALWAYS returns first element
}

char* next() {
  if (names[index] == NULL) index = 0; // set index to 0 when arrived at last element delmiited via NULL
  else index++;

  return names[index];
}

int main() {
  for (char* str = first(); str != NULL; str = next()) {
    printf("%s ", str);
  }

  return 0;
}
```

```text
John Paul George Ringo
```

Iterators like those above are more common in C++ where they are more elegant (also in C there are prettier ways to do this).

Until now we used loops with assignments (state model). It is also possible to have loops in the functional model where they are realized via recursion:

```C
#include <stdio.h>

void say_it(int i) {
  if (i == 0) return;
  printf("Ctuhlu!\n");
  say_it(--i); // i-- would NOT work because i is evaluated -> 3 and passed to the function and THEN decremented -> endless loop
}

int main() {
  say_it(3);
  return 0;
}
```

```text
Ctuhlu!
Ctuhlu!
Ctuhlu!
```

It is also possible to separate the loop body from the "loop mechanic" &rarr; _generic loop function_. To do so we use a _pointer to functions_.

```C
#include <stdio.h>

typedef void (*functionPointer) (int); // functions are passed by pointer (efficiency) need to be wrapped in () else it would be interpreted as "void*" return type (pointer to void) instead

void loop(functionPointer func, int start, int end, unsigned int step) {
  if (start <= end) {
    func(start);
    loop(func, start+step, end, step);
  }
}

void loopBody(int i) {
  printf("%d^2 = %d\n", i, i * i);
}

int main() {
  loop(loopBody, 0, 5, 1);
  return 0;
}

```

```text
0^2 = 0
1^2 = 1
2^2 = 4
3^2 = 9
4^2 = 16
5^2 = 25
```

In Python such things are achievable more easily via [lambda functions](https://www.programiz.com/python-programming/anonymous-function).


## Equivalence {#equivalence}

Generally every loop in a program can be translated into a recursive function and each (trivially) recursive function can be computed via loops. To be precise: _primitive-recursive functions_ can be computed with _a priori limited_ loops, for the so called _μ-recursive functions_ (_my_ "müh" xD as i like to call it). The ACKERMANN-Function for example is not trivially recursive.

So for the most practical use cases loops and recursion are equipotent (gleichmächtig). However loops are usually more clearer (übersichtlicher), but there are also cases where recursion are the handier solution.
