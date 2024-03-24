# Data Structures for Application Programmers



## 1. The Big Picture - When to use what and how much does it cost?


### Perspective

* **The first priority** as an application programmer is to make the code **work** and **clear** to understand.
* **Performance** is important.
* Focusing too much on **performance** can lead to complicated code that is difficult to understand.

### “Big O” Notation

* Generalized metric for measuring performance

* Definition: $T(n)=O(f(n))$ if and only if there exists constants $c > 0$ and $n_0 > 0$ such that $T(n) \leq c \cdot f(n)$, $\forall n \geq n_0$.
![](./res/graph_of_T_n_f_n_and_2f_n.png){ width=50% }

* Characterstics
    * Ignore constants: $1000n = O(n)$
    * Ignore low order terms: $n^3 + n^2 + n = O(n^3)$

### Big-O Classifications

[Time complexity (Big O) cheat sheet](https://leetcode.com/explore/interview/card/cheatsheets/720/resources/4725/)

![](./res/big_o.png){ width=50% }

* $O(1)$: `Constant`
    * An algorithm does NOT depend on the input size.
* $O(log n)$: `Logarithmic`
    * An algorithm gets **slightly slower** as $n$ grows.
* $O(n)$: `Linear`
    * The running time grows as much as $n$ grows (When $n$ doubles, runtime doubles).
* $O(n \cdot log n)$: `Linearithmic`
    * Usually the result of performing $O(log n)$ operation $n$ times or performing $O(n)$ operation $log n$ times.
* $O(n^2)$: `Quadratic`
* $O(2^n)$: `Exponential`
* $O(n!)$: `Factorial`

### Categories of Data Structures

* **General-Purpose Data Structures**
    * Arrays, Linked Lists, Binary Search Trees and Hash Tables.
* **Special-Purpose Data Structures (Abstract Data Types (ADTs))**
    * Stack (LIFO), Queue (FIFO) and Priority Queue
* **Sorting and Searching**
    * Bubble Sort, Selection Sort, Insertion Sort, Quick Sort, Merge Sort, Heap Sort, Linear Search and Binary Search
* **Graphs**

### Flow Chart
![](./res/flowchart.png){ width=50% }


## 2. Collections

### Interface
`java.util.Collection`

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
![](./res/hierarchy.png){ width=50% }


### General Operations
* Addition (Insertion)
* Removal (Deletion)
* Sorting
* Searching
* Iteration (Traversal)
* Copying (Cloning)



## 3. Arrays

### Arrays in Java
* Manipulating data: `clone()`
* Immutable field: `length`, once an array is created, its length is fixed and cannot be changed.

---

```Java
int[] a = { 1, 2, 3, 4, 5 };
int[] b = { 1, 2, 3, 4, 5 };
```
Array `a` and `b` are two instances,

* `a == b` checks reference/identity, returns false;

* `a.equals(b)` of Object class also checks reference/identity, returns false.

---

Hexadecimal representation of hashcode of the memory address will be printed

```Java
System.out.println(c); // the same as c.toString()
System.out.println(c.toString());
```

---
Array indexing conventionally begins at 0:
    `Element's memory location = Array's memory location + Element length * Element`


### Arrays Class

`java.util.Arrays`

* Check equality: `Arrays.equals(a, b)`

* Printing values: `System.out.print(Arrays.toString(c));`

* Sorting: `Arrays.sort(c);`

* Copying

    1. `Arrays.copyOf(sA, length);`

        ```Java
        int[] d = Arrays.copyOf(a, 2);
        // d = [1, 2]

        int[] d = Arrays.copyOf(a, 10);
        // d = [1, 2, 3, 4, 5, 0, 0, 0, 0, 0]
        ```

    2. `System.arraycopy(sA, sI, dA, dI, length);`

        ```Java
        int[] e = new int[5];
        System.arraycopy(a, 0, e, 0, 3);
        // e = [1, 2, 3, 0, 0]
        ```

    3. `clone()`

        ```Java
        int[] g = {1, 2, 3, 4, 5};
        int[] h = g.clone();
        ```

    * The above three ways of copying perform **Shallow Copy/Cloning**, which means it creates a new instance and copies all the fields of the object to that new instance where both are referencing to the same memory in heap memory.
        * Arrays of **primitive data types**: copies the value;
        * Arrays of **objects** or **references**: copies the reference of every single object; modifying a object affects other arrays
            * Write code to perform **Deep Copy**

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

`java.util.ArrayList`

### ArrayList Methods
* `add(object)`: adds a new element to the end.
* `add(index, object)`: inserts a new element at the specified index.
* `set(index, object)`: replaces an existing element at the specified index with the new element.
* `get(index)`: returns the element at the specified index.
* `remove(index)`: deletes the element at the specified index.
* `size()`: returns the number of elements.

### Java ArrayList
* Whenever an instance of ArrayList in Java is created then **by default** the capacity of Arraylist is `10`.

* **Expand** when `add()` is called, but **DOES NOT shrink** when `delete()`.

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

    * If there is issue with unused memory, manually invoke `trimToSize()` method to free up the memory by shrinking size to the number of elements.

* ArrayList used to implement the **doubling-up policy**; In Java 6, there has been a change to be $(oldCapacity * 3) / 2 + 1$.
    * For corner cases where $oldCapacity = 0$ or $oldCapacity = 1$, we must $+ 1$ to expand the array


### Amortized Analysis

`List<Integer> numbers = new ArrayList<Integer>(4);`

| Running time | # of elements | Array length | Allocated dollars | Cost | Saved dollars | Balance |
|--------------|---------------|--------------|-------------------|------|---------------|---------|
| 1            | 1             | 4            | 3                 | 1    | 2             | 2       |
| 1            | 2             | 4            | 3                 | 1    | 2             | 4       |
| 1            | 3             | 4            | 3                 | 1    | 2             | 6       |
| 1            | 4             | 4            | 3                 | 1    | 2             | 8       |
| 5            | 5             | 8            | 3                 | 5    |-2             | 6       |
| 1            | 6             | 8            | 3                 | 1    | 2             | 10      |
| 1            | 7             | 8            | 3                 | 1    | 2             | 12      |
| 1            | 8             | 8            | 3                 | 1    | 2             | 12      |
| 9            | 9             | 16           | 3                 | 9    |-6             | 6       |

* `add(E e)` method has **amortized constant time**
* Latency Issue  
    ![](./res/list-append-time-per-operation-showing-amortised-linear-complexity.png){ width=50% }


### Binary Search
* Prerequisite: **The array should be ordered**.

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
* The **ones' complement** of a binary number is the value obtained by **inverting (flipping) all the bits** in the binary representation of the number.

#### Two's Complement
* The **most significant bit (MSB)**, i.e. the number on the left, is known as the sign bit, which is used to represent whether the number is positive (0) or negative (1).
* The **twos' complement**, which is the negative equivalent of the original binary number, is get by **taking ones' complement** and **adding 1**.

| Bits | Unsigned value | Signed value (Two's complement) | One's Complement | Two's Complement |
|------|----------------|---------------------------------|------------------|------------------|
| 000  | 0              | 0                               | 111              | 000              |
| 001  | 1              | 1                               | 110              | 111              |
| 010  | 2              | 2                               | 101              | 110              |
| 011  | 3              | 3                               | 100              | 101              |
| 100  | 4              | -4                              | 011              | 100              |
| 101  | 5              | -3                              | 010              | 011              |
| 110  | 6              | -2                              | 001              | 010              |
| 111  | 7              | -1                              | 000              | 001              |

* With $n$ bits, we can represent unsigned numbers $0$ to $2^{(n-1)}$.
* With $n$ bits, we can represent signed numbers $–2^{(n-1)}$ to $2^{(n-1)}–1$.
* Therefore, use `mid = l + (r - l)/2` instead of `mid = (l + r)/2` in Binary Search to avoid potential integer overflow issues.


#### Time Complexity of Binary Search

`O(log n)`

* In general, it takes $\lfloor \log_{2} n \rfloor+ 1$ times to split an array in half before it becomes empty.

* $c * \log_{10}(x) = \log_2(x)$ where $c = \frac{\log (10)}{\log (2)} = 3.3219$

* The time complexity of an algorithm with `O(log n)` does not grow proportionally with the input size $n$ but only increases by a constant factor.
    $$
    \begin{align*}
    c \cdot \log_2(2 * n) & = c \cdot (\log_2 2 + \log_2 n) \\
    & = c + c \cdot \log_2 n
    \end{align*}
    $$


### Big O Caveat
When analyzing algorithms using Big O notation, it is essential to consider constants because Big O notation represents an upper bound on the growth rate of an algorithm's complexity.

Consider:
    $$
    \begin{align*}
    T(n) &= n \log n\\
    U(n) &= 50 n
    \end{align*}
    $$

* $n \text{ is small} \Rightarrow T(n) \text{ might be slower than } U(n)$  
* $n \text{ is large} \Rightarrow T(n) \text{ grows more slowly than } U(n)$  

### Array/AL Summary
1. Random access
2. No holes allowed -> Shifts
3. Immutable length -> Memory, latency issue




## 5. LinkedList

### Java LinkedList
* `addFirst(element: Object)`: void
* `addLast(element: Object)`: void
* `getFirst()`: Object
* `getLast()`: Object
* `removeFirst()`: Object
* `removeLast()`: Object

### Types
1. Singly Linked List
2. Doubly Linked List
3. Circular Linked List

### Implementation

```Java
public class LinkedList<AnyType> {
    private Node<AnyType> head;

    // Constructs an empty list
    public LinkedList() {
        head = null;
    }


    // TODO: implement all the methods below.
    ...


    // Static inner class
    public static class Node<AnyType> {
        private AnyType data;
        private Node<AnyType> next;

        // Constructor with data and next node
        Node(AnyType d, Node<AnyType> n) {
            data = d;
            next = n;
        }
    }

}
```

* Types of nested (inner) classes
    * **Static**: do not have access to the instance variables and methods of the outer class directly.
    * **Non-static**:  have access to the instance variables and methods of the outer class.


### Methods

#### addFirst

```Java
// Inserts a new item at the beginning of the list
public void addFirst(AnyType item) {
    head = new Node<AnyType>(item, head);
}
```

#### Traverse to the last node

```Java
Node<AnyType> tmp = head;
while (tmp.next != null) {
    tmp = tmp.next;
}
```

#### addLast

```Java
/**
 * Inserts a new item to the end of the list.
 */
public void addLast(AnyType item) {
    // If the list is empty, insert head
    if (head == null) {
        head = new Node<AnyType>(item, null);
        return;
    }
    
    // Traverse to the last element
    Node<AnyType> tmp = head;
    while (tmp.next != null) {
        tmp = tmp.next;
    }
    
    // Insert to the end of the list
    tmp.next = new Node<AnyType>(item, null);
}

```

#### insertAfter

```Java
/**
 * Finds a node containing the key and inserts a new item after the node.
 */
public void insertAfter(AnyType key, AnyType item) {
    Node<AnyType> tmp = head;

    // Find the location with the given key
    while (tmp != null && !tmp.data.equals(key)) {
        tmp = tmp.next;
    }
    
    // If the key is found
    if (tmp != null) {
        // Insert after the node
        Node<AnyType> newNode = new Node<AnyType>(item, tmp.next);
        tmp.next = newNode;
    }
}
```


#### insertBefore

```Java
/**
 * Finds a node containing the key and inserts a new item before the node.
 * @param key, a key to be found to add a new element
 * @param item, a data to be added into the list
 */
public void insertBefore(AnyType key, AnyType item) {
    // If the list is empty
    if (head == null) {
        return;
    }
    
    // If head has the key
    if (head.data.equals(key)) {
        head = new Node<AnyType>(item, head); // Insert before head
        return;
    }
    
    /*
     * If key is not in the head;
     * Needs to keep track of previous node of current node
     */
    Node<AnyType> prev = null;
    Node<AnyType> cur = head;
    while (cur != null && !cur.data.equals(key)) {
        prev = cur;
        cur = cur.next;
    }
    
    // Found it, add new node before the current node
    if (cur != null) {
        prev.next = new Node<AnyType>(item, cur);
    }
}

```


#### remove

```Java
/**
 * Removes the first occurrence of a key from the list.
 * @param key, a key to be deleted
 */
public void remove(AnyType key) {
    // If the list is empty
    if (head == null) {
        return;
    }
    
    // If head has the key
    if (head.data.equals(key)) {
        head = head.next;
        return;
    }
    
    /*
     * If key is not in the head;
     * Needs to keep track of previous node of current node
     */
    Node<AnyType> prev = null;
    Node<AnyType> cur = head;
    while (cur != null && !cur.data.equals(key)) {
        prev = cur;
        cur = cur.next;
    }
    
    // If current node has the key
    if (cur != null) {
        prev.next = cur.next;
    }
}

```

### Time Complexities
* addFirst: `O(1)`
* insertBefore/insertAfter: `O(n)`
* remove: `O(n)`
* search: `O(n)`


### Iterator

Modifications to the LinkedList class:

```Java
import java.util.*;

public class LinkedList<AnyType> implements Iterable<AnyType> {
    private Node<AnyType> head;

    // Constructs an empty list
    public LinkedList() {
        head = null;
    }


    ...



    /**
     * Iterator implementation that returns iterator object.
     */
    @Override
    public Iterator<AnyType> iterator() {
        return new LinkedListIterator<>();
    }

    /**
     * (Non-static) inner class for LinkedListIterator that implements Iterator interface.
     */
    private class LinkedListIterator implements Iterator<AnyType> {
        private Node<AnyType> nextNode;

        LinkedListIterator() {
            nextNode = head;
        }

        @Override
        public boolean hasNext() {
            return nextNode != null;
        }

        @Override
        public AnyType next() {
            // When there is no next element
            if (!hasNext()) {
                throw new NoSuchElementException();
            }

            AnyType tmp = nextNode.data;
            nextNode = nextNode.next;
            return tmp;
        }
    }
}
```


### ArrayList v.s. LinkedList
ArrayList
* Locate element: `O(1)`
* Perform insertion/deletion: `O(n)`

LinkedList
* Locate element: `O(n)`
* Perform insertion/deletion: `O(1)`