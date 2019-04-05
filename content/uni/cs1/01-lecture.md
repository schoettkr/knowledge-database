+++
title = "Computer Science I - Lecture 01"
author = ["eo shiru"]
date = 2018-10-12T10:35:00+02:00
lastmod = 2019-04-06T00:14:35+02:00
draft = false
+++

## Organization {#organization}

-   Course Page: [tu-chemnitz.de/fritz/course](https://www.tu-chemnitz.de/informatik/friz/Grundl-Inf/)
-   Script: [tu-chemnitz.de/fritz/script](https://www.tu-chemnitz.de/informatik/friz/Grundl-Inf/Scriptum-Druck/)
-   Lecture: Friday 07:30AM - 09:00AM
-   Excercise/Field work: Friday 09:30AM-10:00AM


## Introduction {#introduction}

An algorithm is an ordered sequence of instructions, that can deliver a certain result with a finite amount of steps.

The _binary system_ is a numeral system that represents numbers with 0 and 1. It therefore is base-2 while our common day to day decimal numeral system is base-10, consisting of the 10 digits: 0,1,2,3,4,5,6,8,9. A computer uses the binary numeral system because distinguishing for example between 10 digits is not practical. With the binary system it is much easier because there are just 2 options. This goes back to a circuit either having voltage or not.

There are several methods to convert base-10 to base-2 and vica versa, so I'll just show two here.

Method 1: Binary digit (0 or 1) times 2^n where n = place (zero-indexed):

-   1001 = 1 \* 2^3 + 0 \* 2^2 + 0 \* 2^1 + 1 \* 2^0 = 9
-   111 = 1 \* 2^2 + 1 \* 2^1 + 1 \* 2^0 = 7

Method 2: Divide the number by 2 (split it in half) and ignore the remainder, write down the results from right to left and then write an 1 below each odd number and a 0 below each even number:

-   Example with 9:

| 1 | 2 | 4 | **9** |
|---|---|---|-------|
| 1 | 0 | 0 | 1     |

-   Example with 43:

| 1 | 2 | 5 | 10 | 21 | **43** |
|---|---|---|----|----|--------|
| 1 | 0 | 1 | 0  | 1  | 1      |

Amongst other common numeral systems are the hexadecimal(16) system (0-9, A-F) and the octal(8) system (0-7).

The basic rule to convert into a number system is: value \* base<sup>place</sup> for each cyper/digit.

As C++ is the main programming language in this course a short intro follows:

```c++
#include <iostream>
using namespace std;

int main()
{
  float temp1, temp2, diff;
  cin >> temp1; // console input
  cin >> temp2;
  diff = temp1 - temp2;

  cout << diff; // console output
  return 0;
}
```

`cout` (object of type `ostream`) and `cin` (object of type `istream`) overload the bitshift operators `<<` and `>>`. `istream` and `ostream` are, amongst other things, pooled in `iostream`.


## Training excercises {#training-excercises}

1.) What is a numeral system and which exist? Which one is used in computers?

&rarr; From wikipedia: A numeral system (or system of numeration) is a writing system for expressing numbers; that is, a mathematical notation for representing numbers of a given set, using digits or other symbols in a consistent manner. The same sequence of symbols may represent different numbers in different numeral systems. For example, "11" represents the number three in the binary numeral system (used in computers) and the number eleven in the decimal numeral system (used in common life)

2.) Convert the following numbers to decimal: 1011, 101010, 1FF, A7

-   1011 =  1 \* 2^3 + 0 \* 2^2 + 1 \* 2^1 + 1 \* 2^0 = 8 + 0 + 2 + 1 = 11
-   101010 = 1 \* 2^5 + 0 \* 2^4 + 1 \* 2^3 + 0 \* 2^2 + 1 \* 2^1 + 0 \* 2^0 = 32 + 8 + 2 = 42
-   1FF = 1 \* 15 + 16 \* 15 + 256 \* 1 = 511
-   A7 = 1 \* 7 + 16 \* 10 = 167

3.) Convert the following numbers to binary: 42, 324, BC5, 7D

-   42 = 1 2 5 10 21 42 = 101010
-   324 = 1 2 5 10 20 40 81 162 324 = 101000100
-   BC5 = B = 1011; C = 1100; 5 = 0101 &rarr; = 101111000101
-   7D = 7 = 111; D = 1101 &rarr; 1111101

4.) Convert the following numbers to hexadecimal: 100011, 11011, 255, 256

-   10011 = 0001 = 1; 0011 = 3 = 13
-   11011 = 0001 = 1; 1011 = 11(=B) = 1B
-   255 = FF
-   256 = 100

5.) Add the following binary numbers: 11101 + 1110 and 101001 + 11001

Align the numbers horizontically and add them together in binary, when adding e.g 1+1 = 2 convert the 2 to binary &rarr; 10 and write down the 0 and take the 1 with you to the next column:

a.)

|   |   | 1 | 1 | 1 | 0 | 1 |
|---|---|---|---|---|---|---|
|   | + | 0 | 1 | 1 | 1 | 0 |
| = | 1 | 0 | 1 | 0 | 1 | 1 |

b.)

|   |   | 1 | 0 | 1 | 0 | 0 | 1 |
|---|---|---|---|---|---|---|---|
|   | + | 0 | 1 | 1 | 0 | 0 | 1 |
| = | 1 | 0 | 0 | 0 | 0 | 1 | 0 |

6.) Which range of natural numbers can be represented with 8 bits? What happens if you 1 to the maximum representable number without increasing the available bits?

-   natural numbers = integers >= 0 &rarr; N = {0, 1, 2, 3, ...}
-   8 bits (usually = 1 byte) may represent natural numbers from 0 to 255 (1's in all 8 bit places = 1+2+4+8+16+32+64+128=255)
-   increasing the number by 1 to 256 without increasing the bits would lead to the following representation: 00000000

7.) What would be an option to represent negative numbers (reffering to excercise 6)?

-   The largest bit could represent the sign (e.g 0 = positive and 1 = negative) &rarr; signed magnitude representation. Then numbers from -127 to +127 could be represented with 8 bits (1 2 4 8 16 32 64 sign)
-   Another and most common method is to build the two's complement meaning build the one's complement and add 1 to the result. To build the one's complement you invert all bits (0 to 1 and 1 to 0), C has the bitwise ~ NOT operator for this action. E.g you have: 01001 which is 9 then you invert it to 10110 and add 1 = 10111.

Alternatively (in my opinion less error prone) you could go from right to left and invert every bit AFTER the first 1. Lets say you represent 2 with 5 bits:
00010 &rarr; 11110

Or for 9: ~01001 = 10111
