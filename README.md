<a name="top"></a>


# 17-683 Data Structures for Application Programmers

## Credits
Notes by: [Bill Li](https://leeklee0427.github.io)

Taught by: [Terry Lee](https://www.cs.cmu.edu/~terrylee/)


## Table of Contents
1. [The Big Picture - When to use what and how much does it cost?](#1-the-big-picture---when-to-use-what-and-how-much-does-it-cost)
    - [Big-O Classifications](#big-o-classifications)

2. [Collections](#2-collections)
    - [Java Collection Hierarchy](#java-collection-hierarchy)

3. [Arrays](#3-arrays)
    - [Arrays Class](#arrays-class)

4. [ArrayList and Binary Search](#4-arraylist-and-binary-search)
    - [Java ArrayList](#java-arraylist)
    - [Amortized Analysis](#amortized-analysis)
    - [Binary Search](#binary-search)

5. [LinkedList](#5-linkedlist)
    - [Java LinkedList Implementation](#java-linkedlist-implementation)
    - [LinkedList Methods](#linkedlist-methods)
    - [Iterable/Iterator](#iterableiterator)

6. [Stack (LIFO)](#6-stack-lifo)
    - [Stack Implementation using Array](#stack-implementation-using-array)
    - [Stack Methods using Array](#stack-methods-using-array)

7. [Queue (FIFO)](#7-queue-fifo)
    - [Queue Implementation using Array](#queue-implementation-using-array)
    - [Queue Methods using Array](#queue-methods-using-array)

8. [Simple Sorting](#8-simple-sorting)
    - [Bubble Sort](#bubble-sort)
    - [Selection Sort](#selection-sort)
    - [Insertion Sort](#insertion-sort)

9. [Comparable/Comparator](#9-comparablecomparator)
    - [Comparable](#comparable)
    - [Comparator](#comparator)

10. [Recursion](#10-recursion)
    - [Binary Search (Recursive)](#binary-search-recursive)

11. [Hashing](#11-hashing)

12. [Hash Table](#12-hash-table)

13. [Hashing, HashMap and HashSet in Java](#13-hashing-hashmap-and-hashset-in-java)
    - [HashMap](#hashmap-example)
    - [HashSet](#hashset-example)

14. [Advanced Sorting](#14-advanced-sorting)
    - [Merge Sort](#merge-sort)
    - [Quick Sort](#quick-sort)

15. [Binary Trees, Binary Search Trees](#15-binary-trees-binary-search-trees)

16. [Huffman Coding](#16-huffman-coding)

17. [TreeMap and TreeSet](#17-treemap-and-treeset)

18. [Heaps and HeapSort](#18-heaps-and-heapsortg)






## 1. The Big Picture - When to use what and how much does it cost?


### Perspective

* **The first priority** as an application programmer is to make the code **work** and **clear** to understand.
* **Performance** is important.
* Focusing too much on **performance** can lead to complicated code that is difficult to understand.

### “Big O” Notation

* Generalized metric for measuring performance

* Definition: $T(n)=O(f(n))$ if and only if there exists constants $c > 0$ and $n_0 > 0$ such that $T(n) \leq c \cdot f(n)$, $\forall n \geq n_0$.


![](./res/graph_of_T_n_f_n_and_2f_n.png)


* Characterstics

    - Ignore constants: $1000n = O(n)$

    - Ignore low order terms: $n^3 + n^2 + n = O(n^3)$



### Big-O Classifications

[Time complexity (Big O) cheat sheet](https://leetcode.com/explore/interview/card/cheatsheets/720/resources/4725/)

![](./res/big_o.png)

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
![](./res/flowchart.png)





[Back to Top](#)





## 2. Collections

### Java Collection Interface
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

### Java Collection Hierarchy
![](./res/hierarchy.png)


### General Operations
* Addition (Insertion)
* Removal (Deletion)
* Sorting
* Searching
* Iteration (Traversal)
* Copying (Cloning)





[Back to Top](#)





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

- `a == b` checks reference/identity, returns false;

- `a.equals(b)` of Object class also checks reference/identity, returns false.

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





[Back to Top](#)





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
    ![](./res/list-append-time-per-operation-showing-amortised-linear-complexity.png)


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
| 001  | 1              | -1                              | 110              | 111              |
| 010  | 2              | -2                              | 101              | 110              |
| 011  | 3              | -3                              | 100              | 101              |
| 100  | 4              | -4                              | 011              | 100              |
| 101  | 5              | 3                               | 010              | 011              |
| 110  | 6              | 2                               | 001              | 010              |
| 111  | 7              | 1                               | 000              | 001              |

* With $n$ bits, we can represent unsigned numbers $0$ to $2^{(n-1)}$.
* With $n$ bits, we can represent signed numbers $–2^{(n-1)}$ to $2^{(n-1)}–1$.
* Use `mid = l + (r - l)/2` to avoid potential integer overflow issues.


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






[Back to Top](#)







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

### Java LinkedList Implementation

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
    private static class Node<AnyType> {
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


### LinkedList Methods

#### addFirst - O(1)

```Java
// Inserts a new item at the beginning of the list
public void addFirst(AnyType item) {
    head = new Node<AnyType>(item, head);
}
```

#### Traverse to the last node - O(n)

```Java
Node<AnyType> tmp = head;
while (tmp.next != null) { // Stop at the last node
    tmp = tmp.next;
}
```

#### addLast - O(n)

```Java
/**
 * Inserts a new item to the end of the list.
 */
public void addLast(AnyType item) {
    // If the list is empty, insert head
    if (head == null) {
        //head = new Node<AnyType>(item, null);
        addFirst(item);
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

#### insertAfter - O(n)

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


#### insertBefore - O(n)

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
        //head = new Node<AnyType>(item, head);
        addFirst(item);
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


#### remove - O(n)

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


### Iterable/Iterator

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
* ArrayList:
    - Locate element: `O(1)`
    - Perform insertion/deletion: `O(n)`  

* LinkedList
    - Locate element: `O(n)`
    - Perform insertion/deletion: `O(1)`  





[Back to Top](#)







## 6. Stack (LIFO)

### Conceptual View
* A web browser's back button can be implemented using a stack by mirroring the behavior of a stack in terms of the order of visited pages.
    - When the user visits a webpage, the URL of the page is pushed onto the stack.
    - When the user clicks the back button in the browser, it pops the top URL from the stack.

* Operations
    - push(), push an item onto the top of the stack
    - pop(), pop the item from the top of the stack


### Stack Interface
```Java
public interface StackInterface<AnyType> {
    void push(AnyType item); // O(1)
    AnyType pop(); // O(1)
    AnyType peek(); // O(1)
    boolean isEmpty(); // O(1)
}
```
The above abstract methods in the interface are already `public` `abstract` so there is no need to include the keywords.


### Stack Implementation using Array

```Java
public class ArrayStack<AnyType> implements StackInterface<AnyType> {
    private static final int DEFAULT_CAPACITY = 10; // Constant
    private int top;
    private Object[] elements;

    public ArrayStack(int initialCapacity) {
        if (initialCapacity <= 0) {
            elements = new Object[DEFAULT_CAPACITY];
        } else {
            elements = new Object[initialCapacity];
        }
        // Set the top to be -1, indicating the stack is empty
        top = -1;
    }

    public ArrayStack() {
        this(DEFAULT_CAPACITY); // Calls the constructor with default capacity
    }

    // TODO: implement all the methods here
    ...
}
```


### Stack Methods using Array


#### isEmpty

```Java
/**
 * Checks if the stack is empty in O(1).
 * @return true if it is empty, false otherwise
 */
@Override
public boolean isEmpty() {
    return top == -1;
}
```


#### Push

```Java
/**
 * Pushes a new item onto the top of the stack in O(1).
 * @param item, a new item to add
 * @throws StackException if stack is full
 */
@Override
public void push(AnyType item) {
    if (top == (elements.length - 1)) {
        throw new StackException("Stack is full");
    }
    top = top + 1;
    elements[top] = item;
}
```


#### Pop
```Java
/**
 * Gets and removes the top item in O(1).
 * @return top, item on the top
 * @throws StackException indicating the stack is empty
 */
@Override
public AnyType pop() {
    if (isEmpty()) {
        throw new StackException("Stack is empty");
    }
    @SuppressWarnings("unchecked")
    AnyType item = (AnyType) elements[top];
    elements[top] = null;
    top = top - 1;
    return item;
}
```


- **Casting Up**: casting an object to a *superclass* or *interface* type
    - **No type check is needed** since every instance of the subclass is also an instance of its superclass
- **Casting Down**: casting an object to a *subclass* type
    - **May throw a ClassCastException** at runtime if the object being cast is not actually an instance of the subclass
    - `@SuppressWarnings("unchecked")` annotation is often used when casting down to suppress the compiler warning about unchecked casts


#### Peek
```Java
/**
 * Gets (peeks) the top element but does not delete it in O(1).
 * @return top, item on the top
 * @throws StackException indicating the stack is empty
 */
@SuppressWarnings("unchecked")
@Override
public AnyType peek() {
    if (isEmpty()) {
        throw new StackException("Stack is empty");
    }
    return (AnyType) elements[top];
}
```


### Stack using LinkedList Methods

Treating the first element of the linked list as the top of the stack
```Java
LinkedList<Integer> theStack = new LinkedList<Integer>();

// Push onto the stack
theStack.addFirst(0);

// Pop from the stack
theStack.removeFirst();
```

Also, `addLast()`/`removeLast()` methods can be used to implement a stack that treats the last element of the linked list as the top of the stack.


### Stack Usage: Reverse a String

```Java
/**
 * Simple Application to reverse a String using Stack.
 */
public class Reverser {
    /**
     * Input string.
     */
    private String input;

    /**
     * Constructor with a specific input string.
     * @param str string to reverse
     */
    public Reverser(String str) {
        input = str;
    }

    /**
     * Performs reversal operation and returns a new string.
     * @return reversed string
     */
    public String doReverse() {
        ArrayStack<Character> stack = new ArrayStack<Character>(input.length());
        for (int i = 0; i < input.length(); i++) {
            stack.push(input.charAt(i));
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        return sb.toString();
    }
}
```



[Back to Top](#)



## 7. Queue (FIFO)


### Conceptual View
- **enqueue**: inserting a new item to the back of the queue
- **dequeue**: removing the first item from the queue


### Queue Interface

```Java
public interface QueueInterface<AnyType> {
    void enqueue(AnyType item);  // O(1)
    AnyType dequeue(); // O(1)
    AnyType peekFront(); // O(1)
    boolean isEmpty(); // O(1)
}
```


### Queue Implementation using Array

```Java
public class ArrayQueue<AnyType> implements QueueInterface<AnyType> {
    private static final int DEFAULT_CAPACITY = 6; // Constant
    private Object[] elements; 
    private int front; // The front index
    private int back; // The back index
    private int nItems; // Keeps track of current number of items

    public ArrayQueue() {
        elements = new Object[DEFAULT_CAPACITY]; 
        front = 0;
        back = -1; // Similar to top in Stack
        nItems = 0;
    }

    // TODO: implement all of the core methods here
    ...
}
```

### Queue Methods using Array

#### isEmpty

```Java
/**
 * Checks if the queue is empty or not in O(1).
 * @return true if the queue is empty, false if not
 */
@Override
public boolean isEmpty() {
    return nItems == 0;
}
```

#### isFull

```Java
/**
 * Private helper method to check if queue is full or not.
 * @return true if it is full and false if not
 */
private boolean isFull() {
    return nItems == elements.length;
}
```

#### enqueue

```Java
/**
 * Enqueues a new item to the end of the queue in O(1).
 * @param item, a new item to add
 * @throws RuntimeException indicating the queue is full
 */
@Override
public void enqueue(AnyType item) {
    if (isFull()) {
        throw new RuntimeException("Queue is full");
    }
    back++;

    int index = back % elements.length;
    elements[index] = item;

    nItems++;
}
```

#### dequeue

```Java
/**
 * Gets and deletes an item from the front of the queue in O(1).
 * @return item, the first item in the queue
 * @throws NoSuchElementException indicating the queue is empty
 */
@Override
public AnyType dequeue() {
    if (isEmpty()) {
        throw new NoSuchElementException();
    }

    int index = front % elements.length;

    @SuppressWarnings("unchecked")
    AnyType item = (AnyType) elements[index];
    elements[index] = null; // Deletes the item

    front++;
    nItems--;

    return item;
}
```

#### peekFront

```Java
/**
 * Gets an item from the front of the queue but does not delete it in O(1).
 * @return the first item in the queue
 * @throws NoSuchElementException indicating the queue is empty
 */
@SuppressWarnings("unchecked")
@Override
public AnyType peekFront() {
    if (isEmpty()) {
        throw new NoSuchElementException();
    }
    return (AnyType) elements[front % elements.length];
}
```

### Queue using LinkedList Methods

```Java
LinkedList<Integer> theQueue = new LinkedList<Integer>();

// Enqueue into the queue
theQueue.addLast(5);

// Dequeue from the queue
theQueue.removeFirst();
```



[Back to Top](#)





## 8. Simple Sorting

### swap
```Java
/**
 * A helper method that swaps two values in an integer array.
 */
private static void swap(int[] data, int one, int two) {
    int temp = data[one];
    data[one] = data[two];
    data[two] = temp;
}
```


### Bubble Sort

```Java
/**
 * Bubble sort, O(n^2) in the worst-case.
 * Focus on the largest value!
 * Simple but slow.
 * @param data, an array of int to sort
 */
public static void bubbleSort(int[] data) {
    for (int out = data.length - 1; out >= 1; out--) { // The remaining element is the smallest
        for (int in = 0; in < out; in++) { // The largest elements are bubbled up to the end, no need to check
            if (data[in] > data[in + 1]) {
                swap(data, in, in + 1);
            }
        }
    }
}
```

* Total comparisons: $\frac{(n-1)n}{2}$
    
    - Arithmetic series formula
    - Binomial coefficient: ${n \choose k} = \frac{n!}{k!(n-k)!}$

* Total swaps: $\frac{(n-1)n}{4}$



### Selection Sort

```Java
/**
 * Selection sort, O(n^2).
 * Focus on the smallest value!
 * Faster than bubble sort mainly due to less number of swaps.
 * @param data, an array of int to sort
 */
public static void selectionSort(int[] data) {
    int min;
    for (int out = 0; out < data.length - 1; out++) {
        min = out;
        for (int in = out + 1; in < data.length; in++) {
            if (data[in] < data[min]) {
                min = in;
            }
        }
        // Swap the min value with out index's value
        if (out != min) {
            swap(data, out, min);
        }
    }
}
```

* Less number of swaps


### Insertion Sort

```Java
/**
 * Insertion sort, O(n^2) in the worst case.
 * Best case runntime complexity is O(n).
 * 
 * Sensitive to the input values.
 * Less number of comparisons on average.
 * Uses shifting (copying) instead of swapping (one swap equals to three copies).
 * @param data, an array of int to sort
 */
public static void insertionSort(int[] data) {

    for (int out = 1; out < data.length; out++) {
        int tmp = data[out];
        int in = out;
        /**
         * Loop backward through the sorted section but not necessarily to 0-th
         * On average, go halfway through the sorted section
         */
        while (in > 0 && data[in - 1] >= tmp) {
            data[in] = data[in - 1]; // Shift to right
            in--;
        }

        // INSERT the tmp value into the right position of the sorted section
        if (out != in) {
            data[in] = tmp;
        }
    }
}
```

* Total comparisons: $\frac{n(n-1)}{4}$


[Back to Top](#)



## 9. Comparable/Comparator

### Comparable

* Implement Comparable interface and override `compareTo()`, the default way to compare

* `compareTo()` is used to implement the **natural/default ordering** of the class

* Note that the class's `compareTo()` should be consistent to `equals()` and `hashCode()` such that:
    - If `x.equals(y)` is true, then `x.compareTo(y)` must equal to 0
    - If `x.equals(y)` is true, then `x.hashCode()` must equal to `y.hashCode()`



```Java
public class Card implements Comparable<Card> {
    private String suit;
    private int rank;

    /**
     * Constructs a card with suit and rank.
     * @param suit suit of a card
     * @param rank rank of a card
     */
    public Card(String s, int r) {
        suit = s;
        rank = r;
    }

    /**
     * Returns suit of a card.
     * @return suit
     */
    public String getSuit() {
        return suit;
    }

    /**
     * Returns rank of a card.
     * @return rank
     */
    public int getRank() {
        return rank;
    }

    /**
     * Returns String representation of card objects.
     * @return String representation of card objects
     */
    @Override
    public String toString() {
        return suit + ", " + rank;
    }

    /**
     * Uses rank to compare cards
     * @return positive, 0 or negative
     */
    @Override
    public int compareTo(Card other) {
        return Integer.compare(rank, other.rank);
    }


    /******************************************************************************
     *
     * Example of compareTo, equals and hashCode methods using Apache Commons Lang
     *
     ******************************************************************************/

    @Override
    public int compareTo(Card other) {
        return new CompareToBuilder()
                .append(this.suit, other.suit)
                .append(this.rank, other.rank)
                .toComparison();
    }

    @Override
    public boolean equals(Object other) {
        if (!(other instanceof Card)) {
            return false;
        }
        Card castOther = (Card) other;
        return new EqualsBuilder()
                .append(this.suit, castOther.suit)
                .append(this.rank, castOther.rank)
                .isEquals();
    }

    @Override
    public int hashCode() {
        return new HashCodeBuilder().append(this.suit).append(this.rank).toHashCode();
    }
    

    /************************************************************************
     *
     *  Example of compareTo, equals and hashCode methods using Google Guava
     *
     *  Note: When using Java 7 or later version, you can use Objects class
     *  in java.util instead of using Guava
     *
     ************************************************************************/

    @Override
    public int compareTo(Card other) {
        return ComparisonChain.start()
                .compare(this.suit, other.suit)
                .compare(this.rank, other.rank)
                .result();
    }

    @Override
    public boolean equals(Object obj){
        if(!(obj instanceof Card)){
            return false;
        }
        Card other = (Card) obj;
        return Objects.equal(this.suit, other.suit)
                && Objects.equal(this.rank, other.rank);
    }

    @Override
    public int hashCode(){
        return Objects.hashCode(this.suit, this.rank);
    }
    
}
```






### Comparator

* Implement Comparator interface and override `compare()`

* `compare()` allows for defining multiple **custom/alternative orderings** for a class


```Java
public class CompareBySuit implements Comparator<Card> {
    /**
     * Implementation of compare method by suit.
     * @return positive, 0 or negative
     */
    @Override
    public int compare(Card x, Card y) {
        return x.getSuit().compareToIgnoreCase(y.getSuit());
    }
}
```


```Java
public class CompareBySuitRank implements Comparator<Card> {
    /**
     * Implementation of compare method using both suit and rank.
     * @return positive, 0, or negative
     */
    @Override
    public int compare(Card x, Card y) {
        int suitResult = x.getSuit().compareToIgnoreCase(y.getSuit());
        if (suitResult != 0) {
            return suitResult;
        }
        return Integer.compare(x.getRank(), y.getRank());
    }

}
```


### Collections.sort

```Java
// Default: uses compareTo method implemented in Comparable class
Collections.sort(cards);

// Uses compare method implemented in Comparator class
Collections.sort(cards, new CompareBySuit());
Collections.sort(cards, new CompareBySuitRank());
```




[Back to Top](#)







## 10. Recursion



### Characteristics

1. **Self-referential Functionality:** A recursive function is distinguished by its ability to call itself, enabling the function to operate on smaller subsets of the problem.

2. **Base Case:** This acts as a termination criterion, preventing infinite recursion and ensuring that each recursive call progresses toward this condition.

3. **Bookkeeping Overhead:** Recursive calls involve considerable management of function states and return addresses, which can lead to increased memory usage.



### Iterative/Recursive Comparison
```Java
/**
 * Sums from 1 to n iteratively.
 * @param n, positive integer
 * @return sum from 1 to n
 */
public static int sum(int n) {
    int result = 0;
    for (int i = 1; i <= n; i++) {
        result = result + i;
    }
    return result;
}
```


```Java
/**
 * Sums from 1 to n recursively.
 *
 * recSum(5)                               15
 *    5+recSum(4)                     10+5=15
 *      4+recSum(3)               6+4=10
 *        3+recSum(2)         3+3=6
 *          2+recSum(1) = 1+2=3
 * @param n
 * @return sum from 1 to n
 */
public static int recSum(int n) {
    if (n == 1) {
        return n;               // base case
    }
    return n + recSum(n - 1); // recursive case
}
```


### Fibonacci Numbers

```Java
/**
 * Finds n-th fibonacci number.
 * @param n, positive value or 0
 * @return n-th fibonacci number
 */
public static long fib(int n) {
    if (n == 0 || n == 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```

Below is the recursion tree for `fib(6)`:
![](./res/fibonacci_diagram.png)


### Binary Search (Recursive)

```Java
/**
 * Binary search in a non-null array.
 * @param data, int array to search against (ordered in ascending order)
 * @param key, int key value to search for
 * @return index if found, -1 if not found
 */
public static int find(int[] data, int key) {
    return find(data, key, 0, data.length - 1);
}
```

```Java
/**
 * Private helper method for binary search that uses recursion.
 * @param data, int array to search against (ordered in ascending order)
 * @param key, int key value to search for
 * @param lowerBound, lower bound index of the array
 * @param upperBound, upper bound index of the array
 * @return index if found, -1 if not found
 */
private static int find(int[] data, int key, int lowerBound, int upperBound) {
    // Base case 1: not found
    if (lowerBound > upperBound) {
        return -1;
    }

    int mid = lowerBound + (upperBound - lowerBound) / 2;

    // Base case 2: found
    if (data[mid] == key) {
        return mid;
    }

    // Recursive cases
    if (data[mid] < key) {
        return find(data, key, mid + 1, upperBound);
    }
    return find(data, key, lowerBound, mid - 1);
}
```


[Back to Top](#)



## 11. Hashing

### Overview

| Data Structure   | Search Method   | Time Complexity |
|------------------|-----------------|-----------------|
| Unordered array  | Linear search   | $O(n)$          |
| Ordered array    | Binary search   | $O(\log n)$     |
| Array            | Random access   | $O(1)$          |

**Hashing** transforms input data into a fixed-size string of bytes.

- Improves the speed of searching

### Conceptual

1. **Digit Addition**
    - `cats` is stored at $3 + 1 + 20 + 19 = 43$
    - `zzzzzzzzzzz` is stored at $26 * 10 = 260$
        - Approximately $50,000/260$ words at the same index

2. **Polynomial Hashcode**
    - `cats` is stored at $3*27^3 + 1*27^2 + 20*27^1 + 19*27^0 = 60,337$
    - `zzzzzzzzzzz` is stored at $26*27^9 + 26*27^8 + 26*27^7 + \dots + 26*27^0$
        - Such array is impossible - Integer overflow
        - Most array indices are empty - Waste of memory

3. **Modulus (%)**
    - Information loss
    - Collisions become inevitable due to the Pigeonhole Principle

---

### Workarounds for Collisions
1. Open Addressing
2. Separate Chaining (Closed Addressing)

#### 1. Open Addressing
- **Linear Probing**: searches sequentially through the table until an empty slot is found.
    - Problem: **Primary Clustering**

- **Quadratic Probing**: searches using a quadratic function.
    - Problem: **Secondary Clustering**

- **Double Hashing**:
    - Hash function 1 $\rightarrow$ Initial index
    - Hash function 2 $\rightarrow$ Step size

#### 2. Separate Chaining (Closed Addressing)
- Separate Chaining creates a linked list at each slot to store all elements that hash to that particular slot.
    - Load factor ($\alpha$) can rise above 1

---

### Load factor ($\alpha$)

- Performance degrades seriously if beyond **two-thirds** full

$$ \alpha = \frac{n}{N} = \frac{\text{number of items}}{\text{length of the array}}$$


### Rehashing
- Rehashing: creates a new array twice as large and rehashes everything.
    - Amortized constant time



[Back to Top](#)





## 12. Hash Table

### Hashing Steps
Object -> int -> hashtable (array)

Two steps:
1. Convert: `hashCode()`
2. Compress: %

### Operations
- search(int key)
- delete(int key)
- insert(int key)


### Implementation

```Java
public class HashTable implements HashTableInterface {

    private static final DataItem DELETED = new DataItem(-1);
    
    private DataItem[] hashArray;
    
    public HashTable(int initialCapacity) {
        hashArray = new DataItem[initialCapacity];
    }

    /**
     * Returns true when the key is found.
     * @param key to search for
     * @return true if found, false if not found
     */
    @Override
    public boolean search(int key) {
        int hashVal = hashFunc(key);
        while (hashArray[hashVal] != null) {
            // If the key is found
            if (hashArray[hashVal].key == key) {
                return true;
            }
            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        return false;
    }

    /**
     * Deletes and returns the int key found in the table.
     * @param key to delete
     * @return deleted int from the table (if not found, -1)
     */
    @Override
    public int delete(int key) {
        int hashVal = hashFunc(key);

        while (hashArray[hashVal] != null) {    
            // If the key is found
            if (hashArray[hashVal].key == key) {
                DataItem item = hashArray[hashVal];
                hashArray[hashVal] = DELETED;
                return item.key;
            }

            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        return -1;
    }

    /**
     * Inserts a positive int key to the table.
     * @param key to insert (positive and unique)
     */
    @Override
    public void insert(int key) {
        int hashVal = hashFunc(key);

        // Search for an empty slot or a reusable slot, flagged as DELETED
        while ((hashArray[hashVal] != null) && (hashArray[hashVal] != DELETED)) {
            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        hashArray[hashVal] = new DataItem(key);
    }

    /**
     * Rudimentary modular hashing function.
     * @param key for which hash value need to be calculated (only positive integers)
     * @return index/hash value of the specified key
     */
    private int hashFunc(int key) {
        return key % hashArray.length;
    }

    /**
     * Static nested class for DataItem.
     */
    private static class DataItem {
        private int key;
        DataItem(int k) {
            key = k;
        }
    }

}
```

### Aspects of Good Hash Functions

1. Deterministic
2. Randomness
    1. Quick computation - Machine-dependent
    2. Evenly distributed - Data-dependent


#### 1. Random key values
- `index = key % hashArray.length;`

#### 2. Non-Random key values

Example: Car-part number

- No guarantee random between 0 to 999-999-99-99-99-9-999

    ```
    033-400-03-94-05-0-535

    - Digits 0-2: Supplier number (1 to 999, currently up to 70)
    - Digits 3-5: Category code (100, 150, 200, 250, up to 850)
    - Digits 6-7: Month of introduction (1 to 12)
    - Digits 8-9: Year of introduction (00 to 99)
    - Digits 10-11: Serial number (1 to 99, never exceeds 100)
    - Digits 12: Toxic risk flag (0 or 1)
    - Digits 13-15: Checksum (sum of the other fields)
    ```

- Generate a range of more random numbers
    1. **Remove Non-Data**:
        - Squeeze key values
            - E.g. Category code should be squeezed to 0-15
        - Remove non-data values
            - E.g. Checksum is derived and does not incorporate any new information

    2. **Use All Data**: Incorporate all available data, avoiding truncation of key values.

    3. **Use Prime Number for Modulo Base**: Select a prime number as the modulo base (hash table length), which prevents clustering of keys that are multiples of the base, promoting a more uniform distribution of hash values.

    4. **Use Folding**: Divide keys into groups of digits and summing up the groups, which enhances randomness and reduces collisions in the hash table.
        - Example: SSN `123-45-6789`
            - For table length $1009$
                - Break into groups of three digits: $123 + 456 + 789 = 1368 \% 1009 = 359$
            - For table length $101$
                - Break into groups of two digits: $12 + 34 + 56 + 78 + 9 = 189 \% 101 = 88$

[Back to Top](#)



## 13. Hashing, HashMap and HashSet in Java


### Java HashCode

```Java
Integer itr = Integer.valueOf(17683);
itr.hashCode();
// Output: 17683

String str = new String("17683");
str.hashCode();
// Output: s.charAt(0) * 31^(n-1) + s.charAt(1) * 31^(n-2) + ... + s.charAt(n-1)
```

- Previously: `cats` is $3*27^3 + 1*27^2 + 20*27^1 + 19*27^0$
- In Javadoc: `cats` is $99*31^3 + 97*31^2 + 116*31^1 + 115*31^0$



### Why 31?


#### ASCII

A-Z (65-90), a-z (97-122)

![](./res/probability-of-digits.png)

- The fifth bit indicates "lowercase"
    - The uppercase 'B' (ASCII 66) is `01000010` in binary.
    - The lowercase 'b' (ASCII 98) is `01100010` in binary.

- In the articles, more lowercase letters than uppercase letters


#### Comparison of Base 32 and 31

For base of 32:

- `ab` is $97 * 32 + 98$
- Since $32 = 2^5$, perform `97 * 32 + 98` = `97 << 5 + 98`
    - Prbability of fifth bit is not spreaded:

        ```
        110000100000      --> 97 << 5
        +    1100010      --> 98
             △△
        ------------
        110010000010
        ```

For base of 31:

- `ab` is $97 * 31 + 98$
- Since $32 = 2^5$, perform `97 * 31 + 98` = `97 << 5 - 97 + 98`
    - Spreading out the bits:

        ```
            110000100000  --> 97 << 5
        -        1100001  --> 97
             △
        ----------------
            101110111111
        +        1100010  --> 98
        ----------------
            110000100001
        ```


### Analysis of indexFor and hash methods

In Java 7 HashMap:

```Java
int hash = hash(key.hashCode());
int i = indexFor(hash, table.length);
```

#### indexFor

```Java
static int indexFor(int h, int length) {
    return h & (length - 1);
}
```

Reason:

- Modulus is slow
- Bitwise operation is fast


For example:

- $86 \% 8 = 6$
- $86 AND (8 - 1) = 6$

```
    1010110  <-  86
AND 0000111  <-   7
  ----------
    0000110  <-   6
```

1. Drop the signed bit (Always positive number)
2. Ignore higher-order bits (Not using all the data -> Collisions)


#### hash

```Java
int hash = hash(key.hashCode());
```

- Splits 32 bits hashcode into two 16 bits and perform XOR
    - Folding





### HashMap Example

```Java
Map<String, Integer> freqOfWords = new HashMap<String, Integer>(10, 0.65f);

String[] words = "coming together is a beginning keeping together is progress working together is success"
        .split(" ");

for (String word : words) {
    Integer frequency = freqOfWords.get(word);
    if (frequency == null) {
        frequency = 1;
    } else {
        frequency++;
    }
    freqOfWords.put(word, frequency);
}
```

```Java
Iterator<String> itr = freqOfWords.keySet().iterator();
while (itr.hasNext()) {
    System.out.println(itr.next());
}
System.out.println();

// using enhanced for-loop and keySet method
for (String word : freqOfWords.keySet()) {
    System.out.println(word);
}
System.out.println();

// using enhanced for-loop and values method
for (Integer frequency : freqOfWords.values()) {
    System.out.println(frequency);
}
```



### HashSet Example

Use of dummy object seems a waste of memory?

- HashMap is **highly optimized** for quick lookups
- HashSet is for **ease of maintainence**


```Java
public class HashSet<E> implements Set<E> {
    private ... HashMap<E, Object> map;
    
    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
}
```


```Java
Set<String> distinctWords = new HashSet<String>();
String[] words = "coming together is a beginning keeping together is progress working together is success"
        .split(" ");

for (String word : words) {
    // No duplicate is allowed
    distinctWords.add(word);
}
```

[Back to Top](#)



## 14. Advanced Sorting




### Merge Sort

- Divide and conquer algorithm:
    - Sort the first half using merge sort.
    - Sort the second half using merge sort.
    - Merge the sorted two halves to create the final sorted result.


#### merge

```Java
/**
 * Merges two sorted arrays into one.
 * Precondition: Two input arrays are sorted in ascending order and not null
 * @param left sorted array
 * @param right sorted array
 * @return merged array where elements are sorted
 */
private static int[] merge(int[] left, int[] right) {
    int[] merged = new int[left.length + right.length];
    int indexLeft = 0, indexRight = 0, indexMerged = 0;

    // Traverse and sort values into merged array from left or right
    while (indexLeft < left.length && indexRight < right.length) {
        if (left[indexLeft] < right[indexRight]) {
            merged[indexMerged] = left[indexLeft];
            indexLeft = indexLeft + 1;
        } else {
            merged[indexMerged] = right[indexRight];
            indexRight = indexRight + 1;
        }
        indexMerged = indexMerged + 1;
    }

    // Not all the elements are copied over to merged array
    if (indexLeft < left.length) {
        for (int i = indexLeft; i < left.length; i++) {
            merged[indexMerged] = left[i];
            indexMerged = indexMerged + 1;
        }
    } else {
        for (int i = indexRight; i < right.length; i++) {
            merged[indexMerged] = right[i];
            indexMerged = indexMerged + 1;
        }
    }

    return merged;
}
```

#### mergeSort

```Java
/**
 * Recursive merge sort.
 * Creates new array where values are sorted in ascending order.
 * @param unsorted array
 * @return sorted array
 */
public static int[] mergeSort(int[] unsorted) {
    // Base case
    if (unsorted.length <= 1) {
        return unsorted;
    }

    // Recursive case: divide into two halves
    int mid = unsorted.length / 2;

    int[] left = new int[mid];
    System.arraycopy(unsorted, 0, left, 0, mid);

    int[] right = new int[unsorted.length - mid];
    System.arraycopy(unsorted, mid, right, 0, right.length);

    // sort the left half using merge sort
    left = mergeSort(left);

    // sort the right half using merge sort
    right = mergeSort(right);

    // merge sorted halves into the final result
    return merge(left, right);
}
```

#### Analysis of Merge Sort

Recurrence Relation using substitution method:

```
T(n) = 1                if n <= 1
T(n) = 2T(n/2) + n      if n > 1

2 * T(n/2) + n
=> 4 * T(n/4) + 2n
=> 8 * T(n/8) + 3n
=> 16 * T(n/16) + 4n
...
=> 2^k * T(n/2^k) + k * n
=> n + log_2{n} * n

n/2^k = 1
n = 2^k
k = log_2{n}
```







### Quick Sort

- Partition the array or sub-arrays into left (smaller values) and right (larger values) groups.
- Call itself again to sort the left group.
- Call itself again to sort the right group.


#### quickSort

```Java
/**
 * Quick sort.
 * @param unsorted an array to sort
 */
public static void quickSort(int[] unsorted) {
    quickSort(unsorted, 0, unsorted.length - 1);
}
```

```Java
/**
 * Private helper method for recursive quick sort.
 * Sorts values of input array in ascending order.
 * @param unsorted array
 * @param left boundary index
 * @param right boundary index
 */
private static void quickSort(int[] unsorted, int left, int right) {
    // Base case
    if (left >= right) {
        return;
    }

    // Recursive case
    int pivot = unsorted[right];
    // Partition into left and right subgroups using the pivot value and get index of the pivot value in final position
    int partition = partition(unsorted, left, right, pivot);
    // call itself again to sort the left half
    quickSort(unsorted, left, partition - 1);
    // call itself again to sort the right half
    quickSort(unsorted, partition + 1, right);
}
```

#### partition

```Java
/**
 * Partitions the input array into two parts using the pivot value.
 * @param array to partition
 * @param left index (inclusive)
 * @param right index (exclusive)
 * @param pivot value (not index)
 * @return index of the pivot value after inserted into final position
 */
private static int partition(int[] arr, int left, int right, int pivot) {
    int leftPointer = left - 1;
    int rightPointer = right;

    while (true) {
        // Scanning for out-of-place values
        while (arr[++leftPointer] < pivot);
        while (rightPointer > left && arr[--rightPointer] > pivot);
        if (leftPointer >= rightPointer) {
            break; // Nothing to swap
        }
        // Swap out-of-place values
        swap(arr, leftPointer, rightPointer);
    }
    // Put pivot value into the final location
    swap(arr, leftPointer, right);
    return leftPointer;
}
```

```Java
/**
 * Helper method to swap two elements in the input array.
 * @param data input array to update
 * @param one one index
 * @param two the other index
 */
private static void swap(int[] data, int one, int two) {
    int tmp = data[one];
    data[one] = data[two];
    data[two] = tmp;
}
```


#### Analysis of Quick Sort

In the worst case:

- Number of comparisons: $O(n)$
- Number of swaps: $O(n/2)$

Worst case runtime:  $\frac{n(n-1)}{2}$

```
7654321
765432
76543
7654
765
76
7
```

Choice of pivot value:

- Pseudo-median - value that serves as an estimation or approximation of the true median of a data set
- Otherwise, pick pivot value at **RANDOM**


### Stability Comparison

1. Quick Sort (Not stable, swaps distant elements) => Primitives
2. Merge Sort (Stable) => Objects


### Sorting in Java

In Java 7:

- Primitives => **Dual-pivot Quick Sort**: half time of QuickSort
- Objects => **Tim Sort**: stable, derived from merge sort and insertion sort
- Range < 7 => **Insertion Sort**

[Back to Top](#)






## 15. Binary Trees (Binary Search Trees)


### Overview
- Binary Trees combines the advantage of both ordered arrays and LinkedLists.
    1. Ordered Array
        - Using Binary Search, search in $O(\log n)$
    2. LinkedList
        - insert: $O(1)$
        - delete: $O(1)$


### Binary Trees Terminology

- Root: The node at the top of the tree.

- Parent: When any node (except the root) has exactly one edge running upward to another node. The node above is called parent of the node.

- Child: Any node may have one or more lines running downward to other nodes. These nodes below the given node called its children.

- Leaf: A node that has no children is called a leaf. There can be only one root in a tree but there can be many leaves.

- Level/Height: the level of a particular node refers to how many generations the node is from the root. The root is at level and its children are at level 1 and so on.
    - Count the number of edges
    - Empty binary tree has height of 0.
    - Binary Tree with only the root has height of 0.

- Balanced vs. unbalanced


- A binary tree has nodes that contain a data element, a *left* reference, and a *right* reference. In other words, every node in a binary tree can have at most two children.
- A **full** binary tree is a binary tree where each node has either zero or two children.
- A **complete** binary tree is a binary tree that is completely filled in reading (from left to right) each row with the possible exception of the bottom level.



### Binary Search Tree
The defining characteristic, or the **ordering invariant**, of binary search tree:

- At any node with a key value, k, in a binary search tree
    - all keys of the elements in the left sub-tree must be less than k
    - all keys of the elements in the right sub-tree must be greater than k.
    - Meaning no duplicate keys are allowed.


### Implementation

```Java
public interface BSTInterface {

    /**
     * Searches for the specified key in the tree.
     * @param key key of the element to search
     * @return boolean value indication of success or failure
     */
    boolean find(int key);

    /**
     * Inserts a new element into the tree.
     * @param key key of the element
     * @param value value of the element
     */
    void insert(int key, double value);

    /**
     * Deletes an element from the tree using the specified key.
     * @param key key of the element to delete
     */
    void delete(int key);

    /**
     * Traverses and prints values of the tree in ascending order based on key.
     */
    void traverse();
}
```




```Java
public class BST implements BSTInterface {

    private Node root;
    
    public BST() {
        root = null;
    }


    /**
     * Searches for the specified key in the tree.
     * @param key key of the element to search
     * @return boolean value indication of success or failure
     */
    @Override
    public boolean find(int key) {
        // If empty tree
        if (root == null) {
            return false;
        }

        Node curr = root;
        
        while (curr.key != key) {
            if (curr.key < key) { // Go right if key is larger than current
                curr = curr.right;
            } else { // Go left if key is smaller than current
                curr = curr.left;
            }

            // If not found
            if (curr == null) { 
                return false;
            }
        }
        return true;
    }



    /**
     * Inserts a new element into the tree.
     * @param key key of the element
     * @param value value of the element
     */
    @Override
    public void insert(int key, double value) {
        Node newNode = new Node(key, value);
        // If empty tree
        if (root == null) {
            root = newNode;
            return;
        }

        Node parent = root;
        Node curr = root;

        while (true) {
            // Ignore duplicate keys
            if (curr.key == key) {
                return;
            }

            parent = curr; // update parent
            if (curr.key < key) {
                // Go right if key is larger than current
                curr = curr.right;
                if (curr == null) { // Found a spot
                    parent.right = newNode;
                    return;
                }
            } else {
                // Go left if key is smaller than current
                curr = curr.left;
                if (curr == null) { // Found a spot
                    parent.left = newNode;
                    return;
                }
            }
        }

    }



    /**
     * Deletes an element from the tree using the specified key.
     * @param key key of the element to delete
     */
    @Override
    public void delete(int key) {
        
        // If empty tree
        if (root == null) {
            return;
        }

        Node parent = root;
        Node curr = root;

        boolean isLeftChild = true;

        while (curr.key != key) {
            parent = curr;

            if (curr.key < key) { // Go right
                isLeftChild = false;
                curr = curr.right;
            } else { // Go left
                isLeftChild = true;
                curr = curr.left;
            }

            // Case 1: not found
            if (curr == null) {
                return;
            }
        }

        if (curr.left == null && curr.right == null) {
            // Case 2: a leaf
            
            if (curr == root) {
                root = null;
            } else if (isLeftChild) {
                parent.left = null;
            } else {
                parent.right = null;
            }

        } else if (curr.right == null) {
            // Case 3: one child (left child)

            if (curr == root) {
                root = curr.left;
            } else if (isLeftChild) {
                parent.left = curr.left;
            } else {
                parent.right = curr.left;
            }

        } else if (curr.left == null) {
            // Case 3: one child (right child)

            if (curr == root) {
                root = curr.right;
            } else if (isLeftChild) {
                parent.left = curr.right;
            } else {
                parent.right = curr.right;
            }

        } else {
            // Case 4: two children
            Node successor = getSuccessor(curr);

            if (curr == root) {
                root = successor;
            } else if (isLeftChild) {
                parent.left = successor;
            } else {
                parent.right = successor;
            }

            // Copy the left subtree
            successor.left = curr.left;
        }
    }




    /**
     * Helper method to find the successor of the toDelete node.
     * This tries to find the smallest value of the right subtree
     * of the toDelete node by going down to the left most node in the subtree
     * @param toDelete node to delete
     * @return the successor of the toDelete node
     */
    private Node getSuccessor(Node toDelete) {
        Node successorParent = toDelete;
        Node successor = toDelete;

        // Search from the root of the right subtree
        Node curr = toDelete.right;

        // Move down to left as far as possible
        while (curr != null) {
            successorParent = successor;
            successor = curr;
            curr = curr.left;
        }

        /*
         * Restructure the right subtree if successor is NOT the right child of toDelete
         */
        if (successor != toDelete.right) {
            successorParent.left = successor.right;
            successor.right = toDelete.right;
        }

        return successor;
    }


    /**
     * Traverses and prints values of the tree in ascending order based on key.
     */
    @Override
    public void traverse() {
        StringBuilder sb = new StringBuilder();
        inOrderHelper(root, sb);
        System.out.println(sb);
    }
    
    /**
     * Recursive helper method to traverse the tree.
     * @param toVisit node to visit
     * @param sb stringbuilder instance to append values of the node
     */
    private void inOrderHelper(Node toVisit, StringBuilder sb) {
        if (toVisit != null) {
            inOrderHelper(toVisit.left, sb);
            sb.append("[").append(toVisit.key).append(",").append(toVisit.value).append("]");
            inOrderHelper(toVisit.right, sb);
        }
    }


    /**
     * static nested Node class for Node.
     */
    private static class Node {
        private int key;
        private double value;
        private Node left;
        private Node right;

        Node(int k, double v) {
            key = k;
            value = v;
            left = null;
            right = null;
        }
    }
}
```







## 16. Huffman Coding


### BST Review
- BST can degenerate to LinkedList whose input values are sorted or inversely sorted
    - Thus, the worst-case running time complexity for major operations is $O(n)$, not $O(log n)$.

- Self-Balanced Trees
    - Red-Black Tree
    - AVL Tree



### Fixed Width Encoding Scheme

| Character | Decimal | Binary  |
|-----------|---------|---------|
| A         | 65      | 1000001 |
| B         | 66      | 1000010 |
| C         | 67      | 1000011 |
| ...       | ...     | ...     |
| X         | 88      | 1011000 |
| Y         | 89      | 1011001 |
| Z         | 90      | 1011010 |

- Standard ASCII is a fixed width encoding and each character is encoded with 7 bits.

    - With 7 bits, we can encode $2^7$ different code

    - To encode alphabet A-Z (uppercase only), the smallest number of bit needed is $\log_2 26 = 5$

- Using a fixed width encoding scheme where we use $n$ bits for a character set and we want to store or transmit $m$ characters, we need $m * n$ bits for the entire file.



### BST Usage: Huffman Coding

#### Overview
- Huffman Coding: an algorithm that uses binary tree to compress data.

- Use cases:
    - **File Compression**: Used in ZIP and GZIP formats to reduce file sizes by encoding frequently used characters with shorter codes.
    - **Data Transmission**: Optimizes network data transmission by minimizing the data volume, thus saving bandwidth.
    - **Image Compression**: Integral to the JPEG image compression standard for reducing data size after image transformation and quantization.
    - **Video Compression**: Utilized in MPEG-2 and other video compression standards to encode common pixel patterns efficiently.
    - **Error Correction Codes**: Helps in creating efficient error-correcting codes for communication systems to detect and correct data transmission errors.
    - **Medical Imaging**: Compresses high-resolution medical images to reduce storage demands and facilitate faster network transmission.
    - **Speech Recognition**: Compresses speech data in speech recognition systems to store and transmit more data efficiently.
    - **Cryptographic Functions**: Combines with cryptographic techniques to compress and secure data simultaneously, enhancing transmission security and efficiency.


#### Example
> "SUSIE SAYS IT IS EASY"

| Frequency Table     | Count (Frequency) |
|---------------------|-------------------|
| S                   | 6                 |
| Space               | 4                 |
| I                   | 3                 |
| A                   | 2                 |
| E                   | 2                 |
| Y                   | 2                 |
| T                   | 1                 |
| U                   | 1                 |
| Linefeed            | 1                 |


#### Steps to create a Huffman Tree
1. Create nodes that have two data items of each character and its frequency.
2. Combine lowest two frequency nodes into a tree with a new parent with the sum of their frequencies.
3. Combine lowest two frequency nodes, including the new node we just created, into a tree with a new parent with the sum of their frequencies.
4. Repeat the previous step until we have one tree with all nodes are linked.
5. Label all left branches (edges) with 0 and all right branches (edges) with 1.


Step 1

```
If(1)   U(1)    T(1)    Y(2)    E(2)    A(2)    I(3)    SP(4)   S(6)
```

Step 2

```
T(1)    (2)     Y(2)    E(2)    A(2)    I(3)    SP(4)   S(6)
       /   \
     If(1) U(1)
```

Step 3

```
Y(2)    E(2)    A(2)    (3)     I(3)    SP(4)   S(6)
                       /   \
                     T(1)  (2)
                          /   \
                        If(1) U(1)
```


Step 4

```
A(2)    (3)     I(3)    (4)     SP(4)   S(6)
       /   \           /   \
     T(1)  (2)       Y(2)  E(2)
          /   \
        If(1) U(1)
```


Final Huffman Tree

```
                              Root(24)
                        _________|_________
                      0|                  1|
                     S(6)                 (18)
                                      _____|_____
                                    0|          1|
                                   SP(4)        (14)
                                             ____|____
                                           0|        1|
                                          I(3)       (11)
                                                   ___|___
                                                 0|      1|
                                                 (5)     (6)
                                               __|__    __|__
                                             0|    1| 0|    1|
                                            (3)   A(2) E(2) Y(2)
                                            __|__
                                          0|    1|
                                          T(1)  (2)
                                              __|__
                                            0|    1|
                                            U(1) Lf(1)

```


| Character | Huffman Code | Count |
|-----------|--------------|-------|
| S         | 0            | 6     |
| Space     | 10           | 4     |
| I         | 110          | 3     |
| A         | 1110         | 2     |
| E         | 11110        | 2     |
| Y         | 111110       | 2     |
| T         | 1111110      | 1     |
| U         | 11111110     | 1     |
| Linefeed  | 11111111     | 1     |



#### Comparison with Fixed Width Encoding

| Encoding Method     | Total Number of Bits                                            |
|---------------------|-----------------------------------------------------------------|
| Fixed width encoding| $22 * 7 = 154$                                                  |
| Huffman coding      | $6*1 + 4*2 + 3*3 + 2*4 + 2*5 + 2*6 + 1*7 + 1*8 + 1*8 = 77$      |






[Back to Top](#)



## 17. TreeMap and TreeSet


### TreeSet

```Java
TreeSet<Integer> schedule = new TreeSet<Integer>();
schedule.add(1223);
schedule.add(1430);
schedule.add(1545);
schedule.add(1610);
schedule.add(1705);
schedule.add(2010);
schedule.add(2215);
schedule.add(2320);
schedule.add(2345);

// show the last bus that leaves PGH before 4pm (strictly less than)
System.out.println(schedule.headSet(1600).last());

// show the first bus that leaves PGH after 10pm (greater than or equal to)
System.out.println(schedule.tailSet(2200).first());
```


### TreeMap

```Java
Map<String, Integer> freqOfWords = new HashMap<String, Integer>();
String[] words = "coming together is a beginning keeping together is progress working together is success"
        .split(" ");

for (String word : words) {
    Integer frequency = freqOfWords.get(word);
    if (frequency == null) {
        frequency = 1;
    } else {
        frequency++;
    }
    freqOfWords.put(word, frequency);
}

// no specific order in HashMap
System.out.println("Elements in HashMap: " + freqOfWords);

// print freqOfWords in ascending order
TreeMap<String, Integer> sortedWords = new TreeMap<String, Integer>(freqOfWords);
```



[Back to Top](#)





## 18. Heaps and HeapSort

### Heap Overview

- A heap is a specialized tree-based data structure that satisfies the **shape property** and **heap property**.

    - **Max Heap**
        - In a max heap, the keys of parent nodes are always greater than or equal to those of their children.
        - The largest element is found at the root.
    - **Min Heap**
        - In a min heap, the keys of parent nodes are always less than or equal to those of their children.
        - The smallest element is found at the root.

- Properties

    - **Shape Property**: A heap is a *complete* binary tree.

    - **Heap Property**: Each node's key is either greater than or equal to (max heap) or less than or equal to (min heap) the keys of its children.


- Analyses
    - A heap is NOT a sorted data structure but can be considered as **PARTIALLY ordered**.
    - A heap always have the smallest possible height which is logarithmic.
    - Heaps are useful to keep getting the object with the highest priority.


- Operations
    - **Insert**: Adding a new key to the heap. This is done by adding the new key at the end of the tree (maintaining the shape property) and then adjusting the tree to maintain the heap property, a process known as "heapifying up."
    - **Remove Max/Min**: Removing and returning the root element from the heap (the maximum in a max heap or the minimum in a min heap) and then re-adjusting the heap to maintain the heap structure and property.
    - **Peek**: Returning the value at the root of the heap without removing it, providing quick access to the maximum or minimum element.
    - **Heapify**: A key operation for transforming an unordered list into a heap. This process involves arranging the nodes to maintain the heap properties.

### Insert: Percolate Up
1. Add the new element to the next available position in the tree, ensuring it remains *complete*.
2. If the new element violates the heap-order property of a max-heap, swap it with its parent. Continue this process up the tree until the heap-order is restored.

### Remove Max/Min: Percolate Down
1. Remove the root (the maximum key) and replace it with the last node from the bottom level to maintain a complete tree structure.
2. Swap the newly positioned node with its larger child if it violates the max-heap property. Continue this adjustment process down the tree until the heap order is fully restored.
 





### Implementation


#### Interface

```Java
public interface MaxHeapInterface {
    /**
     * Inserts a new key into a heap in O(log n) time.
     * @param key key to insert
     * @return boolean to check whether it is successfully inserted or not
     */
    boolean insert(int key);

    /**
     * Removes the highest priority key value (maximum key for max heap) in O(log n) time.
     * @return removed key
     * @throws NoSuchElementExcpetion when there is nothing to remove (empty heap)
     */
    int removeMax();
}
```



```Java

public class MaxHeap implements MaxHeapInterface {
    /**
     * An array of Node.
     */
    private Node[] heapArray;
    /**
     * current size of heap array.
     */
    private int currentSize;

   /**
     * Constructs max heap with initial capacity.
     * precondition : initialCapacity > 0 and reasonably large enough
     * @param initialCapacity initial capacity of heap array
     */
    public MaxHeap(int initialCapacity) {
        heapArray = new Node[initialCapacity];
        currentSize = 0;
    }





    ...




    /**
     * static nested class for Node.
     *
     * No references to left and right children
     * because heap is a complete binary tree and we can use an array
     * and indices to find children and their parent
     */
    private static class Node {
        /**
         * Key of node.
         */
        private int key;

        /**
         * Constructs a new node with key.
         * @param k key
         */
        Node(int k) {
            key = k;
        }
    }

    /**
     * A few simple test cases.
     * Building a max heap and use it to sort in descending order (heap sort)
     * @param args arguments
     */
    public static void main(String[] args) {
        MaxHeap theHeap = new MaxHeap(20);

        // initial removeMax method should throw NoSuchElementException
        // theHeap.removeMax();

        // build a max heap
        theHeap.insert(24);
        theHeap.insert(5);
        theHeap.insert(45);
        theHeap.insert(10);
        theHeap.insert(45);
        theHeap.insert(56);
        theHeap.insert(17);
        theHeap.insert(24);
        theHeap.insert(19);
        theHeap.insert(20);

        // Now we can use the heap to sort (heap sort)
        int[] sorted = new int[theHeap.size()];
        for (int i = 0; i < sorted.length; i++) {
            sorted[i] = theHeap.removeMax();
        }

        System.out.println("Sorted in descending order: " + Arrays.toString(sorted));
    }

}
```



#### Insertion

```Java
/**
 * Inserts a new key into a heap in O(log n) time.
 * @param key key to insert
 * @return whether insertion is successful
 */
@Override
public boolean insert(int key) {
    // If the array is full
    if (currentSize == heapArray.length) {
        return false;
    }

    // Insert into the next available index position to make sure the heap is complete
    heapArray[currentSize] = new Node(key);

    // Restore the heap-order property
    percolateUp(currentSize);
    currentSize = currentSize + 1;

    return true;
}
```



#### Percolate Up

```Java
/**
 * Helper method to percolate up for insert operation to restore heap-order property of Max Heap.
 * @param index starting index
 */
private void percolateUp(int index) {
    Node bottom = heapArray[index];
    int parent = (index - 1) / 2;

    // Compare with parent and move it down if necessary
    while (index > 0 && heapArray[parent].key < bottom.key) {
        heapArray[index] = heapArray[parent]; // move parent's node down
        index = parent; // index goes up
        parent = (parent - 1) / 2; // parent also goes up
    }

    // Put the bottom node into the right position
    heapArray[index] = bottom;
}
```


#### removeMax

```Java
/**
 * Removes the highest priority key value (maximum key for max heap) in O(log n) time.
 * @return removed key
 * @throws NoSuchElementExcpetion when there is nothing to remove (empty heap)
 */
@Override
public int removeMax() {
    // If array is empty
    if (currentSize == 0) {
        throw new NoSuchElementException("The heap is empty");
    }

    Node root = heapArray[0];
    currentSize = currentSize - 1;

    // Promote the last node to the root
    heapArray[0] = heapArray[currentSize];
    
    // Remove the last node
    heapArray[currentSize] = null;
    
    // Restore the heap-order property
    percolateDown(0);
    
    return root.key;
}

```

#### Percolate Down

```Java
/**
 * Helper method to percolate down for removeMax operation to restore the heap-order property of Max Heap.
 * @param index starting index
 */
private void percolateDown(int index) {
    Node top = heapArray[index];
    
    // Keep track of larger child
    int largerChild;

    // While there is left child (about half of the nodes are leaves)
    while (index < (currentSize / 2)) {
        int leftChild = (index * 2) + 1;
        int rightChild = leftChild + 1; // or (index + 1) * 2;

        // Check the right child is within the boundary of current size and find larger child
        if ((rightChild < currentSize) && 
                heapArray[leftChild].key < heapArray[rightChild].key) {
            largerChild = rightChild;
        } else {
            largerChild = leftChild;
        }

        // If the top's key is bigger than or equal to the largerChild's key, no need to go down any more
        if (heapArray[largerChild].key <= top.key) {
            break;
        }

        // Move larger child up
        heapArray[index] = heapArray[largerChild];
        index = largerChild;
    }

    // Insert the top node into the right position
    heapArray[index] = top;
}
```


#### size
```Java
/**
 * Returns the current size of heap array.
 * @return current size
 */
public int size() {
    return currentSize;
}
```



### Heap Sort

<div style="border: 1px solid black; padding: 10px; margin: 10px;">
  <p>
  
  Procedure
  1. Build a heap using `insert()` method.
  2. Call `removeMax()` for max heap or `removeMin()` for min heap repeatedly to get items in a sorted order.

  </p>
</div>



```Java
int[] array = {4, 10, 3, 5, 1}; // Example array
Heap theHeap = new Heap(array.length); // Initialize the heap with the size of the array

// Build the heap from the array elements
for (int i = 0; i < array.length; i++) {
    theHeap.insert(array[i]); // Insert element into the heap
}

// Extract elements from the heap to sort the array
for (int i = array.length - 1; i >= 0; i--) {
    array[i] = theHeap.removeMax(); // Remove the largest element for sorting in ascending order
}

// The array is now sorted in ascending order
```

- The `insert()` and `removeMax()` methods of a heap both operate with a worst-case time complexity of $O(\log n)$;

- Since each of these operations is applied n times (once for each element in the array), the entire heap sort process runs in $O(n \log n)$ time.



### Priority Queue
- The PriorityQueue class in Java implements a binary heap, which is a min-heap by default.
    - `add(object)` or `offer(object)`: $O(\log n)$
    - `poll()`: $O(\log n)$
    - `peek()`: $O(1)$
    - `remove(object)`: $O(n)$

```Java
import java.util.PriorityQueue;
import java.util.Comparator;

// Define a class to represent a patient
class Patient {
    String name;
    int priority;  // Higher numbers indicate higher priority

    // Constructor for patient
    public Patient(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }

    // Override toString to display patient information nicely
    @Override
    public String toString() {
        return name + " (" + priority + ")";
    }
}

// Main class for the emergency room scenario
public class EmergencyRoom {

    public static void main(String[] args) {
        // Comparator for the Patient class to make PriorityQueue a max-heap based on patient priority
        Comparator<Patient> patientPriorityComparator = new Comparator<Patient>() {
            @Override
            public int compare(Patient p1, Patient p2) {
                return Integer.compare(p2.priority, p1.priority);  // Reverse order for max-heap
            }
        };

        // Create a PriorityQueue with the custom comparator
        PriorityQueue<Patient> erQueue = new PriorityQueue<>(patientPriorityComparator);

        // Adding patients to the ER queue
        erQueue.add(new Patient("Alice", 2));
        erQueue.add(new Patient("Bob", 5));
        erQueue.add(new Patient("Charlie", 1));
        erQueue.add(new Patient("Dana", 4));

        // Processing patients based on priority
        System.out.println("Patients are being treated in the order of their priority:");
        while (!erQueue.isEmpty()) {
            Patient nextPatient = erQueue.poll(); // Retrieves and removes the highest priority patient
            System.out.println("Treating patient: " + nextPatient);
        }
    }
}
```


### Comparison of BST and Heap

#### Use Cases

1. Which tree is for easier searching?

    **BST**: Easier for general searching as it allows for ordered operations like finding the minimum, maximum, predecessor, and successor.

2. Which tree is for retrieving the maximum key quickly?

    **Max Heap** (Min Heap for minimum): A max heap is designed to allow quick retrieval of the maximum element.

3. Which tree is guaranteed to be complete or almost complete?

    **Max Heap** (or any Heap): Heaps are always complete binary trees by definition.

4. Which tree is guaranteed to be balanced?

    Neither is guaranteed to be balanced in their basic form. A binary search tree can become unbalanced depending on the order of insertion unless it is a self-balancing binary search tree like an AVL or Red-Black Tree. A heap maintains a complete tree structure but not necessarily a balanced tree height in the sense used for balanced search trees.


#### Time Complexities of Operations

|                  | Binary Search Tree | Max Heap (Min Heap) |
|------------------|---------------------|---------------------|
| **Insertion**    | O(h)                | O(log n)            |
| **Deletion**     | O(h)                | O(log n)            |
| **Search**       | O(h)                | O(n)                |
| **Peek Max (Min)**| N/A               | O(1)                |

- $h$ represents the height of the binary search tree, which in the worst case (unbalanced tree) can be O(n), but for balanced BSTs (like AVL, Red-Black trees), it is O(log n).

- $n$ is the number of elements in the heap or tree.


[Back to Top](#)