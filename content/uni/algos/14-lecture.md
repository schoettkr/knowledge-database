+++
title = "Algos & Programming - Lecture 14"
author = ["eo shiru"]
date = 2018-11-24
lastmod = 2019-04-06T00:17:35+02:00
draft = false
+++

## Design and Correctness of Algorithms {#design-and-correctness-of-algorithms}


### Pseudocode {#pseudocode}

As we've learnt in the last lecture _pseudocode_ is one of the most popular semiformal specification languages.

Pseudocode generally defines a set of instructions, there are no strict rules on how pseudocode needs to look like, therefore there are many forms of it, which is okay as long as it is intuitively readable.

Example pseudocode for the Euclidian Algorithm:

<div style="background:lightgrey;">
  <div></div>

**Require:** A, B &isin; N, A > 0 &cap; B > 0<br />
**Ensure:** a = b = gcd(A, B)<br />
a &larr; A; b &larr; B<br />
**while** a &ne; b **do**<br />
   **if** a < b **then**<br />
      b &larr; b - a<br />
   **else**<br />
      a &larr; a - b<br />
   **end if**<br />
**end while**<br />

</div>

The slides note at this point that in the future we'll often look at algorithms in pseudo code.


### Correctness {#correctness}

When you find an approach for a problem / an algorithm, the _correctness_ has to be validated.

It is advised to verify the correctness of the idea first and then the correctness of the concrete algorithm.
The correctness of an idea cannot be verified formally, but the following things should be pondered:

-   does the idea work _in general_ or just for a _specific case_?
-   are there _special cases_ in which the idea does not work out? are these relevant?
-   try finding an _counter-example_
-   what about _extreme cases_?

What follows on slides 25-30 are some example problems and ideas to solve them, take a look there if you want.

As stated before the correctness of the concrete algorithm also has to be validated. There are in general two ways to do this:

-   **Exhaustive Testing**
    -   verify that there is no incorrect behaviour in the implementation of the algorithm by testing/executing it with all possible data inputs/combinations
    -   Problems:
        -   usually this is _impossible_ since the possible data combinations are really large or even infinite
        -   _partial testing_ may boost the confidence in regards to the correctness of a program/algorithm, but does not replace a proof
        -   a good selection of test cases is difficult
-   **Correctness Proof** (Korrektheitsbeweis)
    -   verify correct behaviour via mathematical methods/proofs
    -   Problems:
        -   are difficult or impossible on the level of implementation because of lacking formalization

To proof the correctness it can be performed on a more _abstract level_ instead. Mistakes are then however possible when performing the concrete implementation. We'll continue to look at proofs and not tests.


### Proofs {#proofs}

Slides: Informale Definition = Ein Beweis ist eine Herleitung einer Aussage aus bereits bewiesenen Aussagen und/oder Grundannahmen (Axiomen)

We know a few different (but still combinable) methods for proofs:

-   **deduction**
    -   classical proof via combination of premises
    -   eg: all humans are mortal(premise 1) & all kings are humans (premise 2) &rarr; all kings are mortal (conclusion/deduction)
    -   the correctness of the premises has to be given axiomatically or already been proven
-   **complete case analysis/differentiation** (vollständige Fallunterscheidung)
    -   when there are a finite amount of cases/variants then each one can be inspected individually
    -   if a statement is true for _every_ case/variant then the statement is true as a whole
    -   eg: "all odd integers in the intervall [2^1, 2^3] are prime numbers" (statement) &rarr; 3 is prime, 5 is prime, 7 is prime &rarr; statement is true
-   **complete /transfinite induction**
    -   base cases (Induktionsanker/-anfang IA) show that a statement is valid for a special (smallest/first) case (often \\(n = 0\\) or \\(n = 1\\))
    -   step case / inductive step (Induktionsschritt)
        -   assume that the statement holds true for \\(n=k\\) (Induktionsvoraussetzung IV) and prove that then the statements holds for \\(n=k+1\\); proof that the _induction hypothesis_ follows from the _induction requirement_
        -   Induktionsschluss &rarr; inference (Folgerung) that the statement holds for all cases starting at the first
-   **indirect proof**
    -   assume the opposite of the hypothesis/statement and find a disproof via axioms and proven concepts &rarr; inference that the assumption is wrong and therefore the hypothesis/statment is true


### Proofs of Algorithms {#proofs-of-algorithms}

To prove an algorithm you have to ask two questions. 1) What is there to be proven (Specification)? 2.) What is already known?

A distinction is made between 2 kinds of "correctness":

-   **partial correctness**
    -   an algorithm is _partially correct_ if an answer is returned that this answer will be correct (slides: ein Algorithmus is partiell korrekt, wenn er für eine spezifizerte erfüllte Vorbedingung Q bei einer eventuellen/möglichen Terminierung eine spezifizerte Nachbedingung R erreicht, dh R is nach Ausführung erfüllt)
-   **total correctness**
    -   total correctness requires additionally to partial correctness that the algorithm **terminates**

So, _if_ a partial correct algorithm terminates he yields a correct result and a total correct algorithm yields the correct result after a finite amount of time.


#### Proving Sort Algorithms {#proving-sort-algorithms}

We already got to know a sorting algorithm in one of the first lectures (bubble sort). The correctness of a solution to the problem of sorting can be expressed independently from the algorithm: Input = sequence of elements (e\_1, e\_2, ..., e\_n) &rarr; Output = permutation (e'\_1, e'\_2, ..., e'\_n) of (e\_1, e\_2, ..., e\_n) so that e'\_1 &le; e'\_2 &le; ... &le; e'\_n

The bubble sort algorithm would look like this in pseudocode:

<div style="background:lightgrey;">
  <div></div>

**Require:** e\_1, ..., e\_n<br />
**Ensure:** &forall; i &isin; {1, n-1}, e\_i &le; e<sub>i+1</sub><br />

</div>

<div style="background:lightgrey;">
  <div></div>

**repeat**<br />
  _changed_ &larr; false<br />
  **for** _i_ &larr; 1, ..., _n_-1 **do**<br />
   **if** e\_i > e<sub>i+1</sub> **then**<br />
    SWAP(e\_i, e<sub>i+1</sub>)<br />
    _changed_ &larr; true<br />
   **end if**<br />
  **end for**<br />
**until** _changed_ = false<br />

Proving the _partial correctness_ is simple: If this algorithm terminates _changed_ has to be _false_ which implicates that for no i &isin; {1, ..., n-1} this e\_i > e<sub>i+1</sub> can be true which in reverse means that &forall; i &isin; {1, ..., n-1}, e\_i &le; e<sub>i+1</sub>

</div>

[_Insertion Sort_](https://en.wikipedia.org/wiki/Insertion%5Fsort) is an alternative algorithm to solve the sorting problem. This is the corresponding pseudocode:

<div style="background:lightgrey;">
  <div></div>

**Require:** e\_1, ..., e\_n<br />
**Ensure:** &forall; i &isin; {1, n-1}, e\_i &le; e<sub>i+1</sub><br />

</div>

<div style="background:lightgrey;">
  <div></div>

**for** j &larr; 2, ..., n **do**<br />
  _key_ &larr; e\_j<br />
  _i_ &larr; j-1<br />
  **while** (_i_ > 0) &and; (e\_i > key) **do**<br />
   e<sub>i+1</sub> &larr; e\_i           ; move all elements that are greater than _key_ right<br />
   i &larr; i-1<br />
  **end while**<br />
  e\_i+1 &larr; key          ; fill the gap with _key_<br />
**end for**

</div>

Take a look at slides 47 - 50 (chapter 7) for details on how to prove the correctness of insertion sort with lemmas.


### Soundness (Korrektheitskalküle) {#soundness--korrektheitskalküle}

Proofs like the one we saw for bubble sort are _ad hoc_. There are/is a special logic/calculus (Kalküle &rarr; formales System zum Ziehen logischer Schlüsse) in regards to the correctness of programs, for example the **FLoyd-Hoare logic** (Hoare-Kalkül) or the **wp-Kalkül** (Edsger Dijkstra).

These logics/calculus use triples: {Precondition} Code {Postcondition}. There are axiomatic rules: \frac{premise}{consequence}


### Termination {#termination}

**Partial Correctness** is proven under the assumption that the code _terminates_. Therefore termination has to be proven to prove **total correctness**. This is especially critical when dealing with recursion (abort after finite steps and reach the recursion base) and loops (loop condition has to evaluate to false after finite steps and the loop body also has to terminate in each iteration).

To prove termination of a loop a **termination function** &tau; (Tau) has to be specified:
\\[ \tau : V \rightarrow \mathbb{N} \\]

The termination function has to have the following characteristics:

-   its values are natural numbers (including 0)
-   each iteration resp. execution of the loop body **reduces** its value (strictly monotonically decreasing)
-   the loop condition is _false_ when &tau; = 0
-   &tau; is the upper boundary for the loop iterations that are left

If a termination function is known a **termination rule** can be used:
</knowledge-database/images/termination-rule.png >

So **if** a termination function is _strictly monotonically decreasing_ **and** the value 0 leads to the end of the loop **and** the loop body terminates, **then** the loop **terminates**

So this has to be shown:

1.  strictly monotonically decrease of &tau;
2.  the implication that the loop condition B is not met at the lowest &tau;
3.  the termination of the body P

Example of a termination function for a loop that calculates the square of a nonnegative integer:

```C
/* { Input: 0 <= a} */
int y = 0;
int x = 0;

while (y != a) {
  x = x + 2*y + 1;
  y = y + 1;
 }

/* { Output: x = a^2} */
```

Pick the termination function &tau; = a - y

1.  &tau; is decremented in each iteration, since 'y' is incremented and 'a' is constant
2.  if &tau; = 0 then y = a therefore the loop condition y != a evaluates to _false_
3.  the loop body does not contain recursions, gotos or other loops, termination is therefore trivial

&rarr; The loop terminates!

To prove the termination of recursions the same procedure as with loops can be applied. A termination function &tau; is created that gets smaller with increasing recursion depth. The following has to apply:

1.  the values are natural numbers (incl 0)
2.  the value of &tau; decreases with each method call (recursion)
3.  discontinuation is forced at &tau; = 0 (or earlier)

```C
/* Fibonacci Example */
int fib(int n) {
  if (n < 3) return 1;
  else return (fib(n-1) + fib(n-2));
}
```

However proving termination is not always possible (eg golbachs conjecture for expressing integers as sum of primes)
