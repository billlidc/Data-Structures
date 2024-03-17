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
```java.util.Collection```
| Interface | Implementation          |
|-----------|-------------------------|
| List      | ArrayList, LinkedList   |
| Set       | HashSet, TreeSet        |
| Map       | HashMap, TreeMap        |

* **Set** extends Collection but forbids duplicates.
* **List** extends Collection also and allows duplicates and introduces positional indexing.
* **Map** does not extend Collection, but still considered to be part of the Collection Framework.
    * Map is a collection of pairs (key, value).
    * Map cannot contain duplicate keys.

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
* Immutable field: ```length```, once an array is created, its length is fixed and cannot be changed.

---

```Java
int[] a = { 1, 2, 3, 4, 5 };
int[] b = { 1, 2, 3, 4, 5 };
```
Array ```a``` and ```b``` are two instances,
* ```a == b``` checks reference/identity
* ```a.equals(b)``` of the Object class also checks reference/identity

---

Hexadecimal representation of hashcode of the memory address will be printed
```Java
System.out.println(c); // the same as c.toString()
System.out.println(c.toString());
```


### Arrays Class
```java.util.Arrays```

* Check equality: ```Arrays.equals(a, b)```

* Printing values: ```System.out.print(Arrays.toString(c));```

* Sorting: ```Arrays.sort(c);```

* Copying
    1. ```Arrays.copyOf(sA, length);```
        ```Java
        int[] d = Arrays.copyOf(a, 2);
        // d = [1, 2]

        int[] d = Arrays.copyOf(a, 10);
        // d = [1, 2, 3, 4, 5, 0, 0, 0, 0, 0]
        ```

    2. ```System.arraycopy(sA, sI, dA, dI, length);```
        ```Java
        int[] e = new int[5];
        System.arraycopy(a, 0, e, 0, 3);
        // e = [1, 2, 3, 0, 0]
        ```
    3. ```clone()```
        ```Java
        int[] g = {1, 2, 3, 4, 5};
        int[] h = g.clone();
        ```

    * The above three ways of copying perform **Shallow Copy/Cloning**, which means it creates a new instance and copies all the fields of the object to that new instance where both are referencing to the same memory in heap memory.
        * Arrays of **primitive data types**: copies the value;
        * Arrays of **objects** or **references**: copies the reference of every single Object; modifying a object affects other arrays
            * Write code to perform Deep Copy

### Resizing an Array
```Java
// Removes item at specified index and returns a new array;
// If not within bounds, return the same array
public static char[] delete(char[] data, int index) {
    if (index >= 0 && index < data.length) {
        char[] tmp = new char[data.length - 1];
        System.arraycopy(data, 0, tmp, 0, index);
        System.arraycopy(data, index + 1, tmp, index, data.length - index - 1);
        return tmp;
    }
    return data;
}
```


## 4. ArrayList