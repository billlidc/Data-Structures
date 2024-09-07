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




# 2. Collections

## Overview
- **Collection** is a set of items or objects.

## Java Collections
> `java.util.Collection`

- The Java Collections Framework, comprises a sophisticated set of **interfaces** and **classes** designed for handling collections of objects.

    <!-- ![](../res/hierarchy.png) -->
    <img src="../res/hierarchy.png" alt="" width="500">

    - **List** extends (child of) `Collection`
        ```java
        public interface List<E> extends Collection<E>
        ```
        - **Allows duplicates**
        - Introduces positional indexing

    - **Set** extends (child of) `Collection`
        ```java
        public interface Set<E> extends Collection<E>
        ```
        - **Does NOT allow duplicates**

    - **Map** does NOT extend (not child of) `Collection`, but still considered to be part of the Java Collection framework.
        ```java
        public interface Map<K, V>
        ```
        - Collection of key-value pairs.
        - **Does NOT allow duplicate keys**


- Interface Implementations

    | Interface | Implementation          |
    |-----------|-------------------------|
    | List      | ArrayList, LinkedList   |
    | Set       | HashSet, TreeSet        |
    | Map       | HashMap, TreeMap        |



## Common Operations of Collections
- Insert
- Delete
- Sort
- Search
- Traverse
- Copy / Clone


---

[Back to Home](../index.html)
[Next Lecture](./lecture3.html)