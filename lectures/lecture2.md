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
- Collection is a set of items or objects procured or gathered together by a person, group, or other agent.

## Java Collections Framework
- The Java Collections Framework, part of the `java.util` package, comprises a sophisticated set of interfaces and classes designed for handling collections of objects.

    <!-- ![](../res/hierarchy.png) -->
    <img src="../res/hierarchy.png" alt="" width="500">


## Java Collections Interface

> `java.util.Collection`

| Interface | Implementation          |
|-----------|-------------------------|
| List      | ArrayList, LinkedList   |
| Set       | HashSet, TreeSet        |
| Map       | HashMap, TreeMap        |

- **List** extends Collection, allows duplicates and introduces positional indexing.

- **Set** extends Collection and does NOT allow duplicates.

- **Map** does NOT extend Collection, but still considered to be part of the Java Collection framework.
    - Map is a collection of key-value pairs.
    - Map does NOT contain duplicate keys.


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