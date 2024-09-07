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



# 5. LinkedList

## Types of LinkedList
1. Singly Linked List
2. Doubly Linked List
3. Circular Linked List


## Java LinkedList


### Methods
- `addFirst(element: Object): void`
- `addLast(element: Object): void`
- `getFirst(): Object`
- `getLast(): Object`
- `removeFirst(): Object`
- `removeLast(): Object`


### Implementation

```Java
public class LinkedList<AnyType> {
    private Node<AnyType> head;

    // Constructs an empty list
    public LinkedList() {
        head = null;
    }

    ...

    // Static inner class
    private static class Node<AnyType> {
        private AnyType data;
        private Node<AnyType> next;

        // Constructor with data and next node
        Node(AnyType d, Node<AnyType> n) {
            data = d;
            next = n;
        }
    }

}
```

- Inner Classes
    - **Static**: No access to the instance variables and methods of the outer class.
    - **Non-static**: Can access to the instance variables and methods of the outer class.



#### `addFirst(E e)`: O(1)

```Java
// Inserts a new item at the beginning of the list
public void addFirst(AnyType item) {
    head = new Node<AnyType>(item, head);
}
```

#### Traverse: O(n)

```Java
Node<AnyType> tmp = head;
while (tmp.next != null) { // Stop at the last node
    tmp = tmp.next;
}
```

#### `addLast(E e)`: O(n)

```Java
/**
 * Inserts a new item to the end of the list.
 */
public void addLast(AnyType item) {
    // If the list is empty, insert head
    if (head == null) {
        //head = new Node<AnyType>(item, null);
        addFirst(item);
        return;
    }
    
    // Traverse to the last element
    Node<AnyType> tmp = head;
    while (tmp.next != null) {
        tmp = tmp.next;
    }
    
    // Insert to the end of the list
    tmp.next = new Node<AnyType>(item, null);
}

```

#### `insertAfter()`: O(n)

```Java
/**
 * Finds a node containing the key and inserts a new item after the node.
 */
public void insertAfter(AnyType key, AnyType item) {
    Node<AnyType> tmp = head;

    // Find the location with the given key
    while (tmp != null && !tmp.data.equals(key)) {
        tmp = tmp.next;
    }
    
    // If the key is found
    if (tmp != null) {
        // Insert after the node
        Node<AnyType> newNode = new Node<AnyType>(item, tmp.next);
        tmp.next = newNode;
    }
}
```

#### `insertBefore()`: O(n)

```Java
/**
 * Finds a node containing the key and inserts a new item before the node.
 * @param key, a key to be found to add a new element
 * @param item, a data to be added into the list
 */
public void insertBefore(AnyType key, AnyType item) {
    // If the list is empty
    if (head == null) {
        return;
    }
    
    // If head has the key
    if (head.data.equals(key)) {
        //head = new Node<AnyType>(item, head);
        addFirst(item);
        return;
    }
    
    /*
     * If key is not in the head;
     * Needs to keep track of previous node of current node
     */
    Node<AnyType> prev = null;
    Node<AnyType> cur = head;
    while (cur != null && !cur.data.equals(key)) {
        prev = cur;
        cur = cur.next;
    }
    
    // Found it, add new node before the current node
    if (cur != null) {
        prev.next = new Node<AnyType>(item, cur);
    }
}

```

#### `remove(E e)` - O(n)

```Java
/**
 * Removes the first occurrence of a key from the list.
 * @param key, a key to be deleted
 */
public void remove(AnyType key) {
    // If the list is empty
    if (head == null) {
        return;
    }
    
    // If head has the key
    if (head.data.equals(key)) {
        head = head.next;
        return;
    }
    
    /*
     * If key is not in the head;
     * Needs to keep track of previous node of current node
     */
    Node<AnyType> prev = null;
    Node<AnyType> cur = head;
    while (cur != null && !cur.data.equals(key)) {
        prev = cur;
        cur = cur.next;
    }

    // If current node has the key
    if (cur != null) {
        prev.next = cur.next;
    }
}

```


## Iterable/Iterator

```Java
import java.util.*;

public class LinkedList<AnyType> implements Iterable<AnyType> {
    private Node<AnyType> head;

    // Constructs an empty list
    public LinkedList() {
        head = null;
    }

    ...

    /**
     * Iterator implementation that returns iterator object.
     */
    @Override
    public Iterator<AnyType> iterator() {
        return new LinkedListIterator<>();
    }

    /**
     * (Non-static) inner class for LinkedListIterator that implements Iterator interface.
     */
    private class LinkedListIterator implements Iterator<AnyType> {
        private Node<AnyType> nextNode;

        LinkedListIterator() {
            nextNode = head;
        }

        @Override
        public boolean hasNext() {
            return nextNode != null;
        }

        @Override
        public AnyType next() {
            // When there is no next element
            if (!hasNext()) {
                throw new NoSuchElementException();
            }

            AnyType tmp = nextNode.data;
            nextNode = nextNode.next;
            return tmp;
        }
    }
}
```


## ArrayList & LinkedList
| Operation                  | ArrayList             | LinkedList            |
|----------------------------|-----------------------|-----------------------|
| Locate element             | O(1)                  | O(n)                  |
| Perform insertion/deletion | O(n)                  | O(1)                  |


    
---

[Back to Home](../index.html)
[Next Lecture](./lecture6.html)