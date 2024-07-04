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



# 8. Simple Sorting

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


## Bubble Sort

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

- Total comparisons:
    - Arithmetic series formula
        $$\frac{n(n-1)}{2}$$
    - Binomial coefficient
        $${n \choose 2} = \frac{n!}{2!(n-2)!}$$
- Total swaps: $$\frac{n(n-1)}{4}$$



## Selection Sort

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

- Less number of swaps


## Insertion Sort

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

- Total comparisons: $$\frac{n(n-1)}{4}$$



---

[Back to Home](../index.html)
[Next Lecture](./lecture9.html)