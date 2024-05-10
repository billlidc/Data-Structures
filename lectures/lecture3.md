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



# 3. Arrays

## Characteristics of Arrays

- In Java, arrays can store both primitive types and object references, making them effective for organizing sequential data efficiently.

- An array is characterized by `clone()` method for duplication and an immutable field `length`, which defines its capacity.


## Arrays in Java

### Comparison
Array `a` and `b` are two Object instances:

```Java
int[] a = {1, 2, 3, 4, 5};
int[] b = {1, 2, 3, 4, 5};

if (Arrays.equals(a, b)) {
    System.out.println("Arrays with same contents");
}

if (a == b) {
    System.out.println("a == b is true");
} else {
    System.out.println("a == b is false");
}
```

- `a == b` checks reference/identity, returns `false`;
- `a.equals(b)` of the Object class also checks reference/identity, returns `false`.


### toString
```Java
System.out.println(c); // The same as calling c.toString()
System.out.println(c.toString());
```

- Hexadecimal representation of hashcode of the memory address will be printed


### Array Index
Array indexing conventionally begins at 0:

- `Element's memory location = Array's memory location + Element length * Element`


## Static Methods in Arrays Class

> `java.util.Arrays`

### Check equality
```Java
Arrays.equals(a, b);
```

### Printing values
```Java
System.out.print(Arrays.toString(c));
```

### Sorting
```Java
Arrays.sort(c)
```

### Copying
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

- The above three ways perform *Shallow Copy*, which means it creates a new instance and copies all the fields of the object to that new instance where both are referencing to the same memory in heap memory.
    - Arrays of **primitive data types**: copies the value;
    - Arrays of **objects** or **references**: copies the reference of every single object; modifying a object affects other arrays
        - Solution: Adjust the code to perform *Deep Copy*



## Resizing Array

- Once an array is created, the length is fixed and cannot be changed.
- Resize by creating a new array with a different length and copying all or some of the values from the old array to the new array.

```Java
/**
 * Remove item at the specified index and return a new array;
 * If the index is not within bounds, return the array
 */
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


## Worst-Case Efficiencies of Arrays
- Insertion at back: $O(1)$
- Insertion at front: $O(n)$
- Insertion in middle: $O(n)$
- Searching (linear search): $O(n)$
- Deletion: $O(n)$
- Access to an element with its index: $O(1)$


---

[Back to Home](../index.html)