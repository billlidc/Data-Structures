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




[Back to Home](index.html)