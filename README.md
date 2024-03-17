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
        * Arrays of **objects** or **references**: copies the reference of every single object; modifying a object affects other arrays
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


## 4. ArrayList and Binary Search
```java.util.ArrayList```

### ArrayList Methods
* ```add(object)```: adds a new element to the end.
* ```add(index, object)```: inserts a new element at the specified index.
* ```set(index, object)```: replaces an existing element at the specified index with the new element.
* ```get(index)```: returns the element at the specified index.
* ```remove(index)```: deletes the element at the specified index.
* ```size()```: returns the number of elements.

### Java ArrayList
* Whenever an instance of ArrayList in Java is created then **by default** the capacity of Arraylist is ```10```.

* **Expand** when ```add()``` is called, but **DOES NOT shrink** when ```delete()```.
    ```Java
    // Initialize an arraylist with initial length of 0
    List<Integer> numbers = new ArrayList<Integer>(0);

    // Add numbers
    for (int i = 0; i < 10; i++) numbers.add(i);
    System.out.println(numbers);

    // Delete numbers
    for (int i = numbers.size() – 1; i >= 0; i--) {
        if (numbers.get(i) % 2 == 0) numbers.remove(i);
    }
    System.out.println(numbers);
    ```
    If there is issue with unused memory, manually invoke ```trimToSize()``` method to free up the memory by shrinking size to the number of elements.

* ArrayList used to implement the **doubling-up policy**; In Java 6, there has been a change to be $(oldCapacity * 3) / 2 + 1$.
    * For corner cases where $oldCapacity = 0$ or $oldCapacity = 1$, we must $+ 1$ to expand the array


### Amortized Analysis
| Running time | # of elements | Array length | Allocated dollars | Cost | Saved dollars | Balance |
|--------------|---------------|--------------|-------------------|------|---------------|---------|
| 1            | 1             | 4            | 3                 | 1    | 2             | 2       |
| 1            | 2             | 4            | 3                 | 1    | 2             | 4       |
| 1            | 3             | 4            | 3                 | 1    | 2             | 6       |
| 1            | 4             | 4            | 3                 | 1    | 2             | 8       |
| **5**        | **5**         | **8**        | 3                 | 5    |-2             | 6       |
| 1            | 6             | 8            | 3                 | 1    | 2             | 10      |
| 1            | 7             | 8            | 3                 | 1    | 2             | 12      |
| 1            | 8             | 8            | 3                 | 1    | 2             | 12      |
| **9**        | **9**         | **16**       | 3                 | 9    |-6             | 6       |

* ```add(E e)``` method has **amortized constant time**
* Latency Issue  
    ![List Append Time Per Operation](./res/list-append-time-per-operation-showing-amortised-linear-complexity.png)


### Binary Search
* Prerequisite: **The array should already be ordered**.

```Java
public static int binarySearch(int[] data, int key) {
    int l = 0;
    int r = data.length - 1;
    int mid;

    while (true) {
        if (l > r) {
            return -1;
        }
        mid = (l + r)/2; // Overflow issue
        if (data[mid] == key) {
            return mid;
        }
        if (data[mid] < key) {
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
}
```
#### One's Complement
* The ones' complement of a binary number is the value obtained by inverting (flipping) all the bits in the binary representation of the number.

#### Two's Complement
* The most significant bit (MSB), i.e. the number on the left, is known as the sign bit, which is used to represent whether the number is positive (0) or negative (1).
* The twos' complement, which is the negative equivalent of the original binary number, is get by taking ones' complement and adding 1.

| Bits | Unsigned value | One's Complement | Two's Complement | Signed value (Two's complement) |
|------|----------------|------------------|------------------|---------------------------------|
| 000  | 0              | 111              | 000              | 0                               |
| 001  | 1              | 110              | 111              | 1                               |
| 010  | 2              | 101              | 110              | 2                               |
| 011  | 3              | 100              | 101              | 3                               |
| 100  | 4              | 011              | 100              | -4                              |
| 101  | 5              | 010              | 011              | -3                              |
| 110  | 6              | 001              | 010              | -2                              |
| 111  | 7              | 000              | 001              | -1                              |

* With $n$ bits, we can represent unsigned numbers $0$ to $2^{(n-1)}$.
* With $n$ bits, we can represent signed numbers $–2^{(n-1)}$ to $2^{(n-1)}–1$.

#### Tweak
* Change ```mid = (l + r)/2;``` to ```mid = l + (r - l)/2;```