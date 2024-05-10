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



[Back to Home](index.html)
