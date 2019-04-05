+++
title = "Algos & Programming - Lecture 13"
author = ["eo shiru"]
date = 2018-11-19
lastmod = 2019-04-06T00:17:35+02:00
draft = false
+++

## Design and Correctness of Algorithms {#design-and-correctness-of-algorithms}

The first slides of this lecture chapter is just some meta information about the right mindset to create algorithms, which I find to be trivial, that's why I don't repeat that here (slides 1-5).

We usually create a model for a problem (Modellierung) to abstract and reduce it. Especially mathematical concepts are suited as modeling approaches (Modellierungsansätze):

-   sets, multisets
-   permutations
-   trees/hierarchies
-   graphs
-   points (geometry)
-   polygons
-   strings


### Excourse: Graphs {#excourse-graphs}

Graphs are often used for modelling. A graph is a ordered pair \\((V,E)\\), where \\(V\\) is a set of nodes/vertices (Knoten) \\(V = {v\_1, v\_2, v\_3,.., v\_n}\\) and \\(E\\) is a set of edges (Kanten/Linien) \\(E = {e\_1, e\_2, .., e\_m}\\). Depending on the type of graph, \\(E\\) is:

-   in **undirected\*/\*simple** (ungerichtete) graphs **without multiple edges** \\(E\\) is a **2-element subset** of \\(V\\)
-   in **directed** (gerichteten) graphs **without multiple edges** \\(E\\) is a **subset** of all pairs/2-tuples (i,j) which result from the cartesian product of \\(V \* V\\)
-   in **undirected** graphs with "zusammengefassten" **multiple edges** (=Multigraph) \\(E\\) is a **multiset** (Menge die Duplikate erlaubt) "über die Menge \\(W\\)" of all **2-element subsets** of \\(V\\) (?weighted graph?)
-   in **directed** graphs with "zusammengefassten" **multiple edges** (=Multigraph) \\(E\\) is a **multiset**  "über dem kartesischen Produkt \\(V \* V\\)" (?weighted graph?)
-   in **hypergraphs** \\(E\\) is a subset of the power set of \\(V\\)
-   wiki: in gerichteten Graphen mit eigenständigen Mehrfachkanten eine beliebige Menge, deren Elemente mit Hilfe von zwei Funktionen {\displaystyle \mathrm {src} ,\mathrm {tgt} : E&rarr; V} {\mathrm  {src}},{\mathrm  {tgt}}: E&rarr; V die den Elementen einen Quell- bzw. Zielknoten zuordnen, als Kanten angesehen werden (so ein Graph ist dasselbe wie ein Funktor {\displaystyle G: {\mathcal {G}}&rarr; \mathbf {Set} } G: {\mathcal  G}&rarr; {\mathbf  {Set}}, wobei {\displaystyle {\mathcal {G}}} {\mathcal  G} die recht überschaubare Kategorie {\displaystyle {\mathcal {G}}=\\{V{\stackrel {\mathrm {src} }{\longleftarrow }}E{\stackrel {\mathrm {tgt} }{\longrightarrow }}V\\}} {\mathcal  G}=\\{V{\stackrel  {{\mathrm  {src}}}\longleftarrow }E{\stackrel  {{\mathrm  {tgt}}}\longrightarrow }V\\} mit zwei Objekten und zwei ausgezeichneten Pfeilen ist)

Slides: Kanten können Werte (Gewichte) zugewiesen werden, w : E → R. In diesem Fall spricht man von einem gewichteten Graph.

-   _gerichteter_ Graph hat Pfeile an Kanten die die Richtung angeben
-   _gewichteter_ Graph hat Werte an Kanten stehen

Two blog posts on graphs:

-   <https://medium.com/basecs/a-gentle-introduction-to-graph-theory-77969829ead8>
-   <https://medium.com/basecs/from-theory-to-practice-representing-graphs-cfd782c5be38>

The model of the graph is seperate of any concrete data structures. C for example does not have a built-in data type for graphs so you might need to implement one yourself. Furthermore just because a problem was modeled with a specific approach/model does not mean that a respective data structure is always needed to implement the approach.

However if you find yourself in the need of such data structure there are a multitude of approaches to implement it such. Here are two:

-   via **adjacency matrix**:
    -   a square matrix to represent a finite graph
    -   the elements of the matrix indicate whether pairs of vertices are adjacent or not in the graph
    -   in 2d array each element `a_{i,j}` holds information weather or not an edge connects the vertices `v_i` and `v_j` or which weight the edge has:

```C
enum {nodes = 5};

bool a[nodes][nodes];

a[0][1] = true; // -> edge between verticle 0 and verticle 1 exists
```

-   via **structs and pointers**:
    -   vertices can be modelled as structs and edges as pointers inside of those:

```C
enum {maxDegree = 6};

struct node;

typedef struct node {
  int id;
  struct node *in[maxDegree];
  struct node *out[maxDegree];
} node_t;
```

Since graph theory is a relatively old field of mathematics there are a lot of theorems, laws and standard algorithms for solving certain problems.

<div style="color:red;">
  <div></div>

TODO check those code examples

</div>


### Specification {#specification}

<div style="color:red;">
  <div></div>

TODO check those code examples

</div>

An algorithm has to be described somehow. One variant to do so would be in a concrete programming language. The problem with that is that different programming languages know different concepts and no programming language knows all concepts. C for example does not know data types and operations for mathematical sets by itself. Set operations in C could therefore look a bit unclear and obfuscate (verschleiern) the underlying algorithm which is the real point of interest.

A solution to this is writing algorithms in a _specification_ (Spezifikationsprachen) of which there are many (natural languages are too inprecise btw) when the algorithm itself is the point of interest.

There are **formal** and **semiformal** specification languages. The former are automatically processable for example theorem proofs, however these are often hard to read. The latter serve the purpose of communicating about algorithms.

We'll use the probably most popular semiformal specification language for imperative algorithms which is **pseudocode**.

And this (pseudocode) is where we will continue in the next lecture. Cya :)
