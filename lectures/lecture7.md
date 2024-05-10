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


# 7. Queue (FIFO)

## Conceptual View
- `enqueue`: inserting a new item to the back of the queue
- `dequeue`: removing the first item from the queue


## Queue Implementation using Array

```Java
public interface QueueInterface<AnyType> {
    void enqueue(AnyType item);  // O(1)
    AnyType dequeue(); // O(1)
    AnyType peekFront(); // O(1)
    boolean isEmpty(); // O(1)
}
```


```Java
public class ArrayQueue<AnyType> implements QueueInterface<AnyType> {
    private static final int DEFAULT_CAPACITY = 6; // Constant
    private Object[] elements; 
    private int front; // The front index
    private int back; // The back index
    private int nItems; // Keeps track of current number of items

    public ArrayQueue() {
        elements = new Object[DEFAULT_CAPACITY]; 
        front = 0;
        back = -1; // Similar to top in Stack
        nItems = 0;
    }
    
    ...


}
```

## Queue Implementation using Array

### isEmpty

```Java
/**
 * Checks if the queue is empty or not in O(1).
 * @return true if the queue is empty, false if not
 */
@Override
public boolean isEmpty() {
    return nItems == 0;
}
```

### isFull

```Java
/**
 * Private helper method to check if queue is full or not.
 * @return true if it is full and false if not
 */
private boolean isFull() {
    return nItems == elements.length;
}
```

### enqueue

```Java
/**
 * Enqueues a new item to the end of the queue in O(1).
 * @param item, a new item to add
 * @throws RuntimeException indicating the queue is full
 */
@Override
public void enqueue(AnyType item) {
    if (isFull()) {
        throw new RuntimeException("Queue is full");
    }
    back++;

    int index = back % elements.length;
    elements[index] = item;

    nItems++;
}
```

### dequeue

```Java
/**
 * Gets and deletes an item from the front of the queue in O(1).
 * @return item, the first item in the queue
 * @throws NoSuchElementException indicating the queue is empty
 */
@Override
public AnyType dequeue() {
    if (isEmpty()) {
        throw new NoSuchElementException();
    }

    int index = front % elements.length;

    @SuppressWarnings("unchecked")
    AnyType item = (AnyType) elements[index];
    elements[index] = null; // Deletes the item

    front++;
    nItems--;

    return item;
}
```

### peekFront

```Java
/**
 * Gets an item from the front of the queue but does not delete it in O(1).
 * @return the first item in the queue
 * @throws NoSuchElementException indicating the queue is empty
 */
@SuppressWarnings("unchecked")
@Override
public AnyType peekFront() {
    if (isEmpty()) {
        throw new NoSuchElementException();
    }
    return (AnyType) elements[front % elements.length];
}
```

## Queue Implementation using LinkedList

```Java
LinkedList<Integer> theQueue = new LinkedList<Integer>();

// Enqueue into the queue
theQueue.addLast(5);

// Dequeue from the queue
theQueue.removeFirst();
```


---

[Back to Home](../index.html)