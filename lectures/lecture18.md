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


# 18. Heaps and HeapSort

## Heap Overview

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

<div style="border: 1px solid black; padding: 10px; margin: 10px;">
  <p>

  1. Add the new element to the next available position in the tree, ensuring it remains *complete*.
  2. If the new element violates the heap-order property of a max-heap, swap it with its parent. Continue this process up the tree until the heap-order is restored.
  </p>
</div>



### Remove Max/Min: Percolate Down

<div style="border: 1px solid black; padding: 10px; margin: 10px;">
  <p>

  1. Remove the root (the maximum key) and replace it with the last node from the bottom level to maintain a complete tree structure.
  2. Swap the newly positioned node with its larger child if it violates the max-heap property. Continue this adjustment process down the tree until the heap order is fully restored.
  </p>
</div>



## Heap Implementation

### Heap Interface

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


### Insertion

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



### Percolate Up

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


### removeMax

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

### Percolate Down

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


### size
```Java
/**
 * Returns the current size of heap array.
 * @return current size
 */
public int size() {
    return currentSize;
}
```



## Heap Sort

<div style="border: 1px solid black; padding: 10px; margin: 10px;">
  <p>

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



## Priority Queue
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


## Comparison of BST and Heap

### Use Cases

1. Which tree is for easier searching?

    **BST**: Easier for general searching as it allows for ordered operations like finding the minimum, maximum, predecessor, and successor.

2. Which tree is for retrieving the maximum key quickly?

    **Max Heap** (Min Heap for minimum): A max heap is designed to allow quick retrieval of the maximum element.

3. Which tree is guaranteed to be complete or almost complete?

    **Max Heap** (or any Heap): Heaps are always complete binary trees by definition.

4. Which tree is guaranteed to be balanced?

    Neither is guaranteed to be balanced in their basic form. A binary search tree can become unbalanced depending on the order of insertion unless it is a self-balancing binary search tree like an AVL or Red-Black Tree. A heap maintains a complete tree structure but not necessarily a balanced tree height in the sense used for balanced search trees.


### Time Complexities of Operations

|                  | Binary Search Tree | Max Heap (Min Heap) |
|------------------|---------------------|---------------------|
| **Insertion**    | O(h)                | O(log n)            |
| **Deletion**     | O(h)                | O(log n)            |
| **Search**       | O(h)                | O(n)                |
| **Peek Max (Min)**| N/A               | O(1)                |

- $h$ represents the height of the binary search tree, which in the worst case (unbalanced tree) can be O(n), but for balanced BSTs (like AVL, Red-Black trees), it is O(log n).

- $n$ is the number of elements in the heap or tree.


---

[Back to Home](../index.html)
[Next Lecture](./lecture1.html)