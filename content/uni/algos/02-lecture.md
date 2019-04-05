+++
title = "Algos & Programming - Lecture 02"
author = ["eo shiru"]
date = 2018-10-12T14:57:00+02:00
lastmod = 2019-04-06T00:17:13+02:00
draft = false
+++

## "Algorithm" - History & Definition {#algorithm-history-and-definition}

The term "algorithm" goes back to a persian scholar who wrote a book with the latinized title of '**Algoritmi** de numero indorum'. One generic defintion of the term algorithm is that an algorithm is an instruction consisting of an finite amount of well-defined and effective steps (to achieve a certain goal).


### Finitness {#finitness}

Algorithms can be described in different ways, however it has to be guaranteed that their representation is finite, meaning it can be stored on a medium with a limited amount of space and that they have a certain maximum size that is defined.


### Well-defined {#well-defined}

Each step of an algorithm has to be well-defined meaning it has to be a clear instruction that is exactly defined.


### Feasibility / Effectiveness {#feasibility-effectiveness}

Each step of an algorithm should be either directly or via another algorithm be executable.


### Abstraction {#abstraction}

An algorithm shouldn't solve a single problem, but a batch of problems via its abstraced and generalized nature. This achieved via _parameterization_ of the algorithm. That means the algorithm takes a certain input to compute a certain output.


### Termination {#termination}

After an finite amount of steps the algorithm has to finish execution.


### Efficency {#efficency}

Amongst the main criteria of quality of algorithms is their efficiency, meaning that the amount of time they need should be minimal. The amount of time of course relates to the complexity of the problem. To measure this the concept of time complexity is used.


### Determinism (Determinismus, determinstisch) and Determination (Determiniertheit, determiniert) {#determinism--determinismus-determinstisch--and-determination--determiniertheit-determiniert}

-   **Determinism** (Determinismus) = there is only one instruction on how to continue at all times that is defined a priori (&rarr; deterministisch)
    -   non-determinstic algorithms are called randomized algorithms

-   **Determination** (Determiniertheit) = the same input always produces the same output (&rarr; determiniert)

Remember: "Deterministische Algorithmen sind auch stets determiniert, da sie für die gleichen Eingaben auch wieder gleiche Ausgaben liefern. Die Umkehrung gilt jedoch nicht, denn trotz gleichem Ergebnis können dabei unterschiedliche (interne) Zustände durchlaufen werden."


### **Dynamic Finity** (Resource constrained) {#dynamic-finity--resource-constrained}

An algorithm is only allowed to use a finite amount of resources. The space complexity is, similar to the time complexity, another important criteria.

However there are algorithms that do not fulfill some of these criteria. But those are not relevant to us for now because we are interested in algorithms that run on the computer.


## Models {#models}


### Functional model {#functional-model}

The output is a function of the input, the input however is independant of the output (e.g mathematical function).


### State model {#state-model}

Alternatively there is the state model. In the state model no differentation between input and output is done. Data is just in a specific state. Therefore a subsequent state is just a function of the preceeding state. For example: \`d = f(d)\`. In the state model the values of the data can be represendet as a time series.


### Conditional model {#conditional-model}

An algorithm can consist of \`if-then\`-Rules (conditional clauses). The condition may depend on an input (&rarr; Functional model) or a state (&rarr; State model).
