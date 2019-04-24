+++
title = "Computer Science II - Lecture 02"
author = ["eo shiru"]
date = 2019-04-11
lastmod = 2019-04-24T21:33:44+02:00
tags = ["uni", "cs2"]
draft = false
+++

After repeating some pointer basics in the last lecture, we continued with some more pointer stuff in this lecture and also introduced C++ references.


## Pointer to Functions {#pointer-to-functions}

What follows is an example that let's the user choose a function which shall be executed (late binding, dynamic binding)

```C++
#include <iostream>

using namespace std;

int max(int x, int y) {
  return x > y ? x : y; // ternary operator
}

int min(int x, int y) {
  return x < y ? x : y;
}

int main() {
  int a = 1700, b = 1000;
  int (*fp) (int, int); // fp is pointer to a function

  char c;
  cout << "get max (1) or min (0)?";
  cin >> c;

  switch(c) {
  case '0': fp = &min; break;
  case '1': fp = &max; break;
  default: fp = 0;
  }

  if (fp) {
    // dereference the function pointer and call the function
    cout << (*fp)(a, b) << endl;
    // alternatively: implicity type conversion
    cout << fp(a, b) << endl;
  }


}
```


## Dynamic Memory {#dynamic-memory}

I will copy this section over from "Datastructures Lecture 02":


### Memory Management in C++ {#memory-management-in-c}

Many programmers coming to C++ from C are used to doing manual memory management, particularly for string manipulation this is not so needed in C++ like it is in C because there's a `string` type which does not need any explicit allocation. Apart from this miniature (:D) C++ also offers `new` and `delete` for manual memory management. Side note: _Modern C++ code tends to use new quite rarely, and delete very rarely. From a memory standpoint, the disadvantage is that "new" allocates memory off the heap while local objects allocate memory off the stack. Heap allocation times are much slower than allocations off the stack. However, there are still times when it's appropriate to do so, and a solid understanding of how these low-level facilities work can help with understanding of what normally happens "below the hood". There are even times when new and delete are too high-level, and we need to drop back to malloc and free -- but those situations are rare exceptions indeed._<br />
The basic idea of **new** and **delete** is simple: **new** creates an object of a given type and gives a pointer to it, and **delete** destroys an object created by new, given a pointer to it. The reason that new and delete exist in the language is that code often does not know when it is compiled exactly which objects it will need to create at runtime, or how many of them. Thus new and delete expressions allow for "dynamic" allocation of objects. Here's a short example:

```C++
int main() {
  int * p = new int(3);
  int * q = p;
  delete q; // the same as delete p
  return 0;
}
```

For those familiar with the C programming language, `new` is a kind of "type-aware" version of `malloc`: the type of the expression `new int` is `int*`. Hence in C++ where a cast would be necessary to write `int * p = reinterpret_cast<int *>(malloc(sizeof *p));`, no cast is required when using `new`. Because `new` is type-aware, it can also initialize the newly created objects, calling constructors if appropriate. The example above uses this ability to initialize the int created to have the value 3. Another enhancement of new and delete compared to malloc and free is that the C++ standard provides a standard way to change how new and delete allocate memory; in C this is normally achieved using a non-standard technique known as _"interpositioning"_.

Side note: All dynamically allocated memory must be released before the pointer (except smart pointers) pointing to it goes out of scope. So, if the memory is dynamically allocated for a variable within a function, the memory should be released within the function unless a pointer to it is returned or stored by that function.

The basic new and delete operators are intended to allocate only a single object at a time; they are supplemented by `new[]` and `delete[]` for dynamically allocating entire arrays. Uses of `new[]` and `delete[]` are even rarer than uses of basic new and delete; usually a std::vector is a more convenient way to manage a dynamically allocated array.

Note that when you dynamically allocate an array of objects, you must write delete[] when freeing it, not plain delete. Compilers cannot usually give an error if you get this wrong; most likely your code will crash when you run it. Also never free memory that was allocated by `new` with the _C_ `free` function! Since this may lead to corruption of data und expected results.

When a call to delete[] runs, it first retrieves information stored by new[] describing how many elements are present in the dynamically allocated array, and then calls the destructor for each element before deallocating the memory. The actual address of the memory block that was allocated may differ from the value returned by new[] to allow room to store the number of elements; this is one reason why accidentally mixing the array form of new[] with the single-element form of delete may lead to crashes.

Also after deleting pointer via `delete`, using most compilers the pointer variable will still point to the initially reserverd memory location, which can lead to extremely complicated and confusing consequences. That's why pointer variables should be explicit set to `NULL` or `0` after calling `delete` on them to avoid problems.<br />
This is how we'd dynamically create an array with the techniques we just learned:

```C++
int* a = NULL;   // Pointer to int, initialize to nothing.
int n;           // Size needed for array
cin >> n;        // Read in the size
a = new int[n];  // Allocate n ints and save ptr in a.
for (int i=0; i<n; i++) {
    a[i] = 0;    // Initialize all elements to zero.
}
. . .  // Use a as a normal array
delete [] a;  // When done, free memory pointed to by a.
a = NULL;     // Clear a to prevent using invalid memory reference.
```

It isn't strictly necessary to reset a pointer to `NULL`, but it's very good practice so that any use of the pointer will produce an error. Attempts to use memory location 0, which is the normal default value of NULL, will be blocked by the way most operating systems allocate memory. Why doesn't `delete` reset the pointer? It does in some systems, but the language specification does not require it, so not all systems do it.

The "old" C-way of managing dynamic memory shouldnt be used anymore (malloc,..)


### Arrays and Pointers - Example {#arrays-and-pointers-example}

The following code constructs a matrix. However the rows of this matrix are not stored sequentially in order as it would be the case with `int matrix[3][4]` declaration, because we use an array of int pointers. These can point to 1d int arrays somewhere in memory and dont need to be in sequential order:

```C++
#include <iostream>
using namespace std;

int main() {
  int i,j;
  int x,y;
  int **m; //pointer to int-pointer

  cout << "Enter x and y \n";
  cin >> x >> y;

  m = new int*[x]; //Array of x int-pointers

  for (i=0; i < x; i++)
    m[i] = new int[y]; //array of y ints
  for (i = 0; i < x; i++) {
    for (j = 0 ; j < y; j++) {
      cin >> m[i][j];
    }
  }

  for (i = 0; i < x ; i++) {
    for (j = 0 ; j < y; j++) {
      cout << m[i][j]<< ' ';
    }
    cout << endl;
  }

  cout << &m << endl;
  cout << &m[0] << endl;
  cout << &m[0][0] << endl;
  cout << &m[0][1] << endl;
  cout << &m[1] << endl;
  cout << &m[1][0] << endl;
  cout << &m[1][1] << endl;
}
```

&rarr; "Enter  x  and  y"<br />
2 2<br />
1 2 3 4<br />
&rarr; 1   2<br />
&rarr; 3   4<br />
&rarr; 0x7fff15940a20 (&m)<br />
&rarr; 0x561b38f24e90 (&m[0])<br />
&rarr; 0x561b38f24eb0 (&m[0][0])<br />
&rarr; 0x561b38f24eb4 (&m[0][1])<br />
&rarr; 0x561b38f24e98 (&m[1])<br />
&rarr; 0x561b38f24ed0 (&m[1][0])<br />
&rarr; 0x561b38f24ed4 (&m[1][1])


## Pointers and Structs {#pointers-and-structs}

Also copied from Datastructures 02:<br />
In C++ a `typedef` is not needed, thus `struct node { int element; }` is automatically a new _type_ with a member called "element". Quite often structs are accessed/passed via pointers:

```C++
node n[2]; // array of 2 nodes
node* pN = n; // pointer to n = currently pointing to base address of the array n
(*pn).element = 1; // setting element = 1 for first element in the n array
(*(pn + 1)).element = 2; // setting element = 2 for second element in the n array
```

Because of operator precedence `*` to dereference needs to be wrapped in parens when accessing a member of a struct. To avoid this kind of cumbersome syntax, there is the arrow operator `->` which enables the following:

```C++
pn->element = -1;
(pn+1)->element = -2;
```

Another example:

```C++
struct Person {
  char name[20];
  int birthyear;
};

Person max, moritz, leute[10];

max.birthyear = 1993;
leute[0].birthyear = 1900;

int i = 0;
Person* pp = leute;

while (i < 10) {
  pp->birthyear = 1993; // alternative to (*pp).birthyear
  pp++;
  i++;
 }
```

The arrow operator `->` allows accessing struct members that are referenced via pointers.


## References {#references}

Also copied from Datastructures 02:
Pass-by-reference means to pass the reference of an argument in the calling function to the corresponding formal parameter of the called function. The called function can modify the value of the argument by using its reference passed in. Usually or "by default" C++ passes by value.

Pass-by-references is more efficient than pass-by-value, because it does not copy the arguments. The formal parameter is an alias for the argument. When the called function read or write the formal parameter, it is actually read or write the argument itself.

The difference between pass-by-reference and pass-by-value is that modifications made to arguments passed in by reference in the called function have effect in the calling function, whereas modifications made to arguments passed in by value in the called function can not affect the calling function. Use pass-by-reference if you want to modify the argument value in the calling function. Otherwise, use pass-by-value to pass arguments.

The difference between pass-by-reference and pass-by-pointer is that pointers can be NULL or reassigned whereas references cannot. Use pass-by-pointer if NULL is a valid parameter value or if you want to reassign the pointer. Otherwise, use constant or non-constant references to pass arguments.

The following example shows how arguments are passed by reference. The reference parameters are initialized with the actual arguments when the function is called.

```C++
#include <stdio.h>

void swapnum(int &i, int &j) {
  int temp = i;
  i = j;
  j = temp;
}

int main(void) {
  int a = 10;
  int b = 20;

  swapnum(a, b);
  printf("A is %d and B is %d\n", a, b);
  return 0;
}
```

```text
A is 20 and B is 10
```

[Here's](http://www.fredosaurus.com/notes-cpp/functions/refparams.html) a similar example.<br />
References are mainly used when passing/calling by reference and for specifying operations on classes. One more example:

```C++
int i = 1;
int &ri = i; // ri and i refer to the same obj
int x = ri; // x = 1
ri = 2; // -> i = 2, x = 1
```

References can also be returned from functions:

```C++
#include <iostream>
using namespace std;

const int MAX = 1000;
float feld[MAX];

float& element(int index) {
  if (index < 0 || index >= MAX) {
    cout << "Invalid index " << index << endl;
    index = 0;
  }

  return feld[index]; // reference!
}

int main() {
  element(MAX) = 5; // Error Message
  cout << element(0) << endl; // now set to 5
}
```

```text
Invalid index 1000
5
```

However in the example above where we wrote a custom array indexing function, it is still possible to access `feld` via eg `feld[3] = 7`. This shouldnt be possible. To circumvent this we can make use of a _static_ array declaration _inside_ the `element()` function (own scope):

```C++
float& element(int index) {
  static float feld[MAX];
}
```


## Strings - C Style {#strings-c-style}

General remarks about strings in C:

-   end of a character string is designated via '\\0' (one additional byte)
-   string constants are pointers to their first char (type = `char*`)
-   need for enough storage for the respective character strings, else buffer overflow
-   usage via `<cstring>` / `<string.h>`

Common string functions:

-   `int strlen(char* s)` for determining the length of a string
-   `int strcmp(char* s1, char* s2)` for comparing strings (0 when s1=s1, < 0 for s1 < s2, > 0 for s1 > s2)
-   `char* strcpy(char *destination, char* source)` and `char* strcat(char *destination, char* source)` for copying strings

There surely is some more content about strings in C on my blog somewhere :D .


## Strings - C++ Style {#strings-c-style}

In contrast to C strings, C++ strings work in an object oriented way. Since this paradigm hasn't been covered yet in this lecture, the usage of C++ strings will just be shown by an example for now:

```C++
#include<iostream>
#include<string> // Standard-String einschliessen
#include<cstddef> // size_t

using namespace std;

int main() { // Programm mit typischen String-Operationen

  // String-Objekt einString anlegen und mit hallo initialisieren
  // einString kann ein beliebiger Name sein.
  string einString("hallo");

  // String ausgeben
  cout << einString << endl;

  // Beim Vektor waere stattdessen fuer die Ausgabe
  // eine Schleife notwendig, etwa der folgenden Art:
  for(size_t i = 0; i < einString.size(); ++i)
    cout << einString[i];
  cout << endl;

  // String zeichenweise mit Indexpruefung ausgeben. Die Anzahl der
  // Zeichen kann bei Strings auch mit length() ermittelt werden.
  for(size_t i = 0; i < einString.length(); ++i)
    cout << einString.at(i);
  cout << endl;

  // Kopie des Strings einString erzeugen
  string eineStringKopie(einString);
  cout << eineStringKopie << endl; // hallo

  // Kopie durch Zuweisung
  string diesIstNeu("neu!");
  eineStringKopie = diesIstNeu;
  cout << eineStringKopie << endl; // neu!

  // Zuweisung einer Zeichenkette in Anfuehrungszeichen
  eineStringKopie = "Buchstaben";
  cout << eineStringKopie << endl; // Buchstaben

  // Zuweisung nur eines Zeichens vom Typ char
  einString = 'X';
  cout << einString << endl; // X

  // Strings mit dem +=-Operator verketten
  einString += eineStringKopie;
  cout << einString << endl; // XBuchstaben

  // Strings mit dem +-Operator verketten
  einString = eineStringKopie + " ABC";
  cout << einString<< endl; // Buchstaben ABC
  einString = "123" + eineStringKopie;
  cout << einString << endl; // 123Buchstaben

  string::size_type pos = einString.find("BU");
  cout << "Position von BU : " << pos << '\n';
  pos = einString.find("Bu");
  cout << "Position von Bu : " << pos << '\n';

  // Eine Erklaerung gibts erst im Kapitel Ueberladen von Operatoren,
  // aber folgendes geht nicht:
  // einString = "123" + "ABC"; // Fehler!
  string a("Albert"), z("Alberta");
  string b = a;

  if(a == b) cout << a << " == " << b << endl;
  else cout << a << " != " << b << endl;

  if(a < z) cout << a << " < " << z << endl;

  if(z > a) cout << z << " > " << a << endl;

  if(z != a) cout << z << " != " << a << endl;
}
```

```text
hallo
hallo
hallo
hallo
neu!
Buchstaben
X
XBuchstaben
Buchstaben ABC
123Buchstaben
Position von BU : 18446744073709551615
Position von Bu : 3
Albert == Albert
Albert < Alberta
Alberta > Albert
Alberta != Albert
```
