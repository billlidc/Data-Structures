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


[Back to Home](index.html)

