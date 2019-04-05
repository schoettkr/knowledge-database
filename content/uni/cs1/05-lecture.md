+++
title = "Computer Science I - Lecture 05"
author = ["eo shiru"]
date = 2018-11-09
lastmod = 2019-04-06T00:14:39+02:00
draft = false
+++

A type alias is a different name by which a type can be identified. In C++, any valid type can be aliased so that it can be referred to with a different identifier.
It is possible to do type definitions via the following syntax `typedef TYP typename;`, eg `typedef unsigned short ushort;`


## Enumerated Type {#enumerated-type}

Enumerated types are types that are defined with a set of custom identifiers, known as enumerators, as possible values. Objects of these enumerated types can take any of these enumerators as value.

Their syntax is:

```cpp
enum type_name {
  value1,
  value2,
  value3,
  .
  .
} object_names;
```

This creates the type `type_name`, which can take any of `value1`, `value2`, `value3`, ... as value. Objects (variables) of this type can directly be instantiated as `object_names`.

Notice that this declaration includes no other type, neither fundamental nor compound, in its definition. To say it another way, somehow, this creates a whole new data type from scratch without basing it on any other existing type. The possible values that variables of this new type `color_t` may take are the enumerators listed within braces. For example, once the `colors_t` enumerated type is declared, the following expressions will be valid:

```cpp
#include <iostream>
using namespace std;

int main() {
  enum colors_t {red, green, blue} testColor = green, yourFavColor = blue;

  colors_t myFavColor;
  myFavColor = blue;

  if (testColor == green) { // green is equivalent to 1
    cout << "'testColor is " << green << endl;
  }

  if (myFavColor == yourFavColor) cout << "Hey we have the same favorite colors :D" << endl;

  return 0;
}
```

```text
'testColor is 1
Hey we have the same favorite colors :D
```

Values of enumerated types declared with `enum` are implicitly convertible to an integer type, and vice versa. In fact, the elements of such an enum are always assigned an integer numerical equivalent internally, to which they can be implicitly converted to or from. If it is not specified otherwise, the integer value equivalent to the first possible value is `0`, the equivalent to the second is `1`, to the third is `2`, and so on... Therefore, in the data type `colors_t` defined above, `red` would be equivalent to `0`, `green` would be equivalent to `1`, blue to `2`, and so on...


## Arrays {#arrays}

An array is a series of elements of the same type placed in contiguous memory locations that can be individually referenced by adding an index to a unique identifier.

That means that, for example, five values of type int can be declared as an array without having to declare 5 different variables (each with its own identifier). Instead, using an array, the five int values are stored in contiguous memory locations, and all five can be accessed using the same identifier, with the proper index.

A typical declaration for an array in C++ is `type name [elements num];` where `type` is a valid type (such as int, float...), `name` is a valid identifier and the `elements num` (which is always enclosed in square brackets `[]`), specifies the length of the array in terms of the number of elements.

Therefore, the `foo` array, with five elements of type `int`, can be declared as: `int foo [5];`

NOTE: The number of elements within square brackets `[]`, representing the number of elements in the array, must be a _constant expression_, since arrays are blocks of static memory whose size must be determined at compile time, before the program runs.

Elements in an array can be explicitly initialized to specific values when it is declared, by enclosing those initial values in braces `{}`. For example:

```cpp
int foo[5] = { 16, 2, 77, 40, 12071 };
```

The number of values between braces `{}` shall not be greater than the number of elements in the array. For example, in the example above, `foo` was declared having 5 elements (as specified by the number enclosed in square brackets, []), and the braces {} contained exactly 5 values, one for each element. If declared with less, the remaining elements are set to their default values (which for fundamental types, means they are filled with zeroes). For example:

```cpp
int bar[5] = { 10, 20, 30 };
```

Results in bar consisting of `10, 20, 30, 0, 0`.

The initializer can even have no values, just the braces `int baz[5] = { };`.
This creates an array of five int values, each initialized with a value of zero:

When omitting the initialization completely by default, regular arrays of _local scope_ (for example, those declared within a function) are left uninitialized. This means that none of its elements are set to any particular value; their contents are undetermined at the point the array is declared.

Static arrays, and those declared directly in a namespace (outside any function), are always initialized. If no explicit initializer is specified, all the elements are default-initialized (with zeroes, for fundamental types)

When an initialization of values is provided for an array, C++ allows the possibility of leaving the square brackets empty `[]`. In this case, the compiler will assume automatically a size for the array that matches the number of values included between the braces `{}`:

```cpp
int foo[] = { 16, 2, 77, 40, 12071 };
```

To access array values one uses the bracket notation:

```cpp
int foo[] = { 16, 2, 77, 40, 12071 };
foo[0] // -> 16
foo[4] // -> 12071
```

Char arrays are somewhat a special case. There are multiple equivalent ways to define char arrays:

```cpp
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
char greeting[6] = "Hello"; // 6 because of delimiting 0
char greeting[] = "Hello";
```

Functions to work with such strings in C++ are provided via the header file `<cstring>`

-   find out length via `int strlen(char *s)`
-   comparison via `int strcmp(char *s1, char *s2)`
    -   returns 0 for "equal" strings (equal string content)
    -   returns < 0 when `s1` is < than `s2` eg s1 holds "a" which in ascii is smaller than "b" so `strcmp` would return `-1` because that is the difference
    -   returns > 0 when `s1` is > than `s2` eg `s1` holds "c" and `s2` holds "a" then `strcmp` returns `2` because that is the ascii distance
-   copy strings with `char * strcpy(char *destination, char *source)` and `char * strcat(char *destination, char *source)`
    -   `strcpy()` copies a string from source to destination. The function takes two string variables as arguments: the destination, and the source, then returns the updated destination variable.
    -   `strcat()` concatenates two strings. It appends a copy of the source string to the end of the destination string, and then returns the destination string.


## Struct {#struct}

A struct is a type consisting of a sequence of members whose storage is allocated in an ordered sequence (as opposed to union, which is a type consisting of a sequence of members whose storage overlaps). There are many instances in programming where we need more than one variable in order to represent an object. For example, to represent yourself, you might want to store your name, your birthday, your height, your weight, or any other number of characteristics about yourself.

Fortunately, C++ allows us to create our own user-defined aggregate data types. An _aggregate data type_ is a data type that groups multiple individual variables together. One of the simplest aggregate data types is the struct. A **struct** (short for structure) allows us to group variables of mixed data types together into a single unit.

Because structs are user-defined, we first have to tell the compiler what our struct looks like before we can begin using it. To do this, we declare our struct using the `struct` keyword. Here is an example of a struct declaration:

```cpp
struct Employee
{
    short id;
    int age;
    double wage;
};
```

This tells the compiler that we are defining a struct named Employee. The Employee struct contains 3 variables inside of it: a short named `id`, an int named `age`, and a double named `wage`. These variables that are part of the struct are called members (or fields). Keep in mind that Employee is just a declaration -- even though we are telling the compiler that the struct will have member variables, no memory is allocated at this time. By convention, struct names start with a capital letter to distinguish them from variable names.

In order to use the Employee struct, we simply declare a variable of type Employee `Employee john;` . This defines a variable of type `Employee` named `john`. As with normal variables, defining a struct variable allocates memory for that variable.

When we define a variable such as `Employee john`, `john` refers to the entire struct (which contains the member variables). In order to access the individual members, we use the member selection operator `.` (which is a period). Here is an example of using the member selection operator to initialize each member variable:

```cpp
Employee john; // create an Employee struct for John
john.id = 14; // assign a value to member id within struct john
john.age = 32; // assign a value to member age within struct john
john.wage = 24.15; // assign a value to member wage within struct john
```

As with normal variables, struct member variables are not initialized, and will typically contain junk. We must initialize them manually.

Initializing structs by assigning values member by member is a little cumbersome, so C++ supports a faster way to initialize structs using an initializer list. This allows you to initialize some or all the members of a struct at declaration time.

```cpp
struct Employee
{
    short id;
    int age;
    double wage;
};

Employee john = { 1, 32, 60000.0 }; // john.id = 1, john.age = 32, john.wage = 60000.0
Employee frank = { 2, 28 }; // frank.id = 2, frank.age = 28, frank.wage = 0.0 (default initialization)
```

If the initializer list does not contain an initializer for some elements, those elements are initialized to a default value (that generally corresponds to the zero state for that type). In the above example, we see that frank.wage gets default initialized to 0.0 because we did not specify an explicit initialization value for it.

---

Source(s): <https://www.learncpp.com/cpp-tutorial/47-structs/>
