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



[Back to Home](index.html)
