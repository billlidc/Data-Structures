# Data Structures for Application Programmers



## 1. The Big Picture - When to use what and how much does it cost?


### Perspective

* **The first priority** as an application programmer is to make the code **work** and **clear** to understand.
* **Performance** is important.
* Focusing too much on **performance** can lead to complicated code that is difficult to understand.

### “Big O” Notation

* Generalized metric for measuring performance

* Definition: $T(n)=O(f(n))$ if and only if there exists constants $c > 0$ and $n_0 > 0$ such that $T(n) \leq c \cdot f(n)$, $\forall n \geq n_0$.
![Graph of T(n), f(n) and 2f(n)](./res/graph_of_T_n_f_n_and_2f_n.png)

* Characterstics
    * Ignore constants: $1000n = O(n)$
    * Ignore low order terms: $n^3 + n^2 + n = O(n^3)$

### Big-O Classifications

[Time complexity (Big O) cheat sheet](https://leetcode.com/explore/interview/card/cheatsheets/720/resources/4725/)
![Big O](./res/big_o.png)

* $O(1)$: ```Constant```
    * An algorithm does NOT depend on the input size.
* $O(log n)$: ```Logarithmic```
    * An algorithm gets **slightly slower** as $n$ grows.
* $O(n)$: ```Linear```
    * The running time grows as much as $n$ grows (When $n$ doubles, runtime doubles).
* $O(n \cdot log n)$: ```Linearithmic```
    * Usually the result of performing $O(log n)$ operation $n$ times or performing $O(n)$ operation $log n$ times.
* $O(n^2)$: ```Quadratic```
* $O(2^n)$: ```Exponential```
* $O(n!)$: ```Factorial```

### Categories of Data Structures

* **General-Purpose Data Structures**
    * Arrays, Linked Lists, Binary Search Trees and Hash Tables.
* **Special-Purpose Data Structures (Abstract Data Types (ADTs))**
    * Stack (LIFO), Queue (FIFO) and Priority Queue
* **Sorting and Searching**
    * Bubble Sort, Selection Sort, Insertion Sort, Quick Sort, Merge Sort, Heap Sort, Linear Search and Binary Search
* **Graphs**

### Flow Chart
![Flow Chart](./res/flowchart.png)


## 2. Collections

### Interface
Java Collection: ```java.util.Collection```
| Interface | Implementation          |
|-----------|-------------------------|
| List      | ArrayList, LinkedList   |
| Set       | HashSet, TreeSet        |
| Map       | HashMap, TreeMap        |

* **Set** extends Collection but forbids duplicates.
* **List** extends Collection also and allows duplicates and introduces positional indexing.
* **Map** does not extend Collection, but still considered to be part of the Collection Framework. Map is a collection of pairs (key, value). Map cannot contain duplicate keys.

### Hierarchy
![Hiearachy](./res/hierarchy.png)


### General Operations
* Addition (Insertion)
* Removal (Deletion)
* Sorting
* Searching
* Iteration (Traversal)
* Copying (Cloning)



## 3. Arrays

### Arrays in Java
* Manipulating data: ```clone()```
* Immutable field: ```length```



## 4. ArrayList