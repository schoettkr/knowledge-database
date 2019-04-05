+++
title = "Computer Science I - Lecture 02"
author = ["eo shiru"]
date = 2018-10-19
lastmod = 2019-04-06T00:14:35+02:00
draft = false
+++

There wasn't much new stuff in this lecture because a wrong announcement was made leading to students missing the first week's lecture. Therefore we did mostly repetitions that I'll skip now and summarize the additional slides.


## Data types in C++ {#data-types-in-c}

The data type constitutes three things:

-   memory mapping
-   codomain/target set (Wertevorrat)
-   valid operations

Standard data types:

-   `int` = whole integers, usually 4 byte
-   `float` = real numbers, usually 4-8 byte
-   `char` = character (in single quotes!), usually 1 byte
-   `bool` logical/boolean values (true and false)


### Type conversion {#type-conversion}

Type conversion is done by calling the type on the object:

```C++
int one = 1;
float(one)/2;
=> 0.5
```
