<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>



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



[Back to Home](index.html)