+++
title = "Computer Science I - Lecture 10"
author = ["eo shiru"]
date = 2018-12-14
lastmod = 2019-04-06T00:14:40+02:00
draft = false
+++

## Development Cycle {#development-cycle}

We're still in the realm of the development cycle (see post to last week's lecture) and continue with the next step which is "Implementation".


### Implementation {#implementation}

Implementation is the step where the draft from the other steps is brought into a format that can be processed by the machine/computer. There's a concrete technical basis that has to be considered:

-   detailed language specification ("detaillierter Sprachumfang")
-   implementation specifications ((co)domains, etc.)
-   operating system
-   usable standard software (eg libraries for arithmetic, graphics, string & list manipulation and data management)


### Translation & Integration {#translation-and-integration}

Here're some possible procedures when translating source code from programming languages:

![](/knowledge-database/images/translation-1.png)
![](/knowledge-database/images/translation-2.png)


### (Software) Quality Assurance {#software--quality-assurance}

Another crucial step in the development cycle is **Quality Assurance** (QA). It is good to prove the correctness of the program. To do so (or come close to that) testcases with predefined test data should run. The expected results should be known beforehand and then be compared with the actual results to prove correctness.

There are different levels of proving correctness:

1.  the program is (syntactically) translated without errors (and no crashes when ran)
2.  existence of certain test data that is handled correctly by the program
3.  typical test data is correctly handled
4.  uncommon and conciously chosen critical or odd test data (eg extreme values) are handled correctly
5.  all possible input values are handled correctly
6.  additionally: meaningful response to typical cases of incorrect data
7.  additionally: meaningful response to all possible cases

In practice point 4./6. are goals that should be targeted to realize.

Quality assurance has to happen in _all_ development design phases:

-   specification &rarr; coordination with users
-   design &rarr; review by experts
-   implementation &rarr; source code reviews by other developers and/or mathematical proofs
-   program &rarr; correctness proof by executing the program and tests (see above)


## Algorithms and Data Structures {#algorithms-and-data-structures}

This is the last big/major chapter in the script.


### Recursive Functions {#recursive-functions}

The pages in the script cover the fibonacci sequence and the towers of hanoi which we already looked at in lectures from "Algorithms & Programming". So you can read the related blog posts to that and/or look at the CS01 slides from page 71 to 77. I won't repeat that stuff again here :P
