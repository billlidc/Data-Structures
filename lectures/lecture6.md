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



# 6. Stack (LIFO)

## Conceptual View

- A web browser's back button can be implemented using a stack by mirroring the behavior of a stack in terms of the order of visited pages.
    - When the user visits a webpage, the URL of the page is pushed onto the stack.
    - When the user clicks the back button in the browser, it pops the top URL from the stack.

- Operations
    - `push()`, push an item onto the top of the stack
    - `pop()`, pop the item from the top of the stack


## Stack Interface

```Java
public interface StackInterface<AnyType> {
    void push(AnyType item); // O(1)
    AnyType pop(); // O(1)
    AnyType peek(); // O(1)
    boolean isEmpty(); // O(1)
}
```
- Methods in the interface are already `public` and `abstract`


## Stack Implementation using Array

```Java
public class ArrayStack<AnyType> implements StackInterface<AnyType> {
    private static final int DEFAULT_CAPACITY = 10; // Constant
    private int top;
    private Object[] elements;

    public ArrayStack(int initialCapacity) {
        if (initialCapacity <= 0) {
            elements = new Object[DEFAULT_CAPACITY];
        } else {
            elements = new Object[initialCapacity];
        }
        // Set the top to be -1, indicating the stack is empty
        top = -1;
    }

    public ArrayStack() {
        this(DEFAULT_CAPACITY); // Calls the constructor with default capacity
    }

    // TODO: implement all the methods here
    ...
}
```


### isEmpty

```Java
/**
 * Checks if the stack is empty in O(1).
 * @return true if it is empty, false otherwise
 */
@Override
public boolean isEmpty() {
    return top == -1;
}
```


### Push

```Java
/**
 * Pushes a new item onto the top of the stack in O(1).
 * @param item, a new item to add
 * @throws StackException if stack is full
 */
@Override
public void push(AnyType item) {
    if (top == (elements.length - 1)) {
        throw new StackException("Stack is full");
    }
    top = top + 1;
    elements[top] = item;
}
```


### Pop
```Java
/**
 * Gets and removes the top item in O(1).
 * @return top, item on the top
 * @throws StackException indicating the stack is empty
 */
@Override
public AnyType pop() {
    if (isEmpty()) {
        throw new StackException("Stack is empty");
    }
    @SuppressWarnings("unchecked")
    AnyType item = (AnyType) elements[top];
    elements[top] = null;
    top = top - 1;
    return item;
}
```

- **Casting Up**: casting an object to a *superclass* or *interface* type
    - **No type check is needed** since every instance of the subclass is also an instance of its superclass

- **Casting Down**: casting an object to a *subclass* type
    - **May throw a ClassCastException** at runtime if the object being cast is not actually an instance of the subclass
    - `@SuppressWarnings("unchecked")` annotation is often used when casting down to suppress the compiler warning about unchecked casts


### Peek
```Java
/**
 * Gets (peeks) the top element but does not delete it in O(1).
 * @return top, item on the top
 * @throws StackException indicating the stack is empty
 */
@SuppressWarnings("unchecked")
@Override
public AnyType peek() {
    if (isEmpty()) {
        throw new StackException("Stack is empty");
    }
    return (AnyType) elements[top];
}
```


## Stack Implementation using LinkedList

Treating the first element of the linked list as the top of the stack
```Java
LinkedList<Integer> theStack = new LinkedList<Integer>();

// Push onto the stack
theStack.addFirst(0);

// Pop from the stack
theStack.removeFirst();
```

Also, `addLast()`/`removeLast()` methods can be used to implement a stack that treats the last element of the linked list as the top of the stack.


## Stack Usage: Reverse a String

```Java
/**
 * Simple Application to reverse a String using Stack.
 */
public class Reverser {
    /**
     * Input string.
     */
    private String input;

    /**
     * Constructor with a specific input string.
     * @param str string to reverse
     */
    public Reverser(String str) {
        input = str;
    }

    /**
     * Performs reversal operation and returns a new string.
     * @return reversed string
     */
    public String doReverse() {
        ArrayStack<Character> stack = new ArrayStack<Character>(input.length());
        for (int i = 0; i < input.length(); i++) {
            stack.push(input.charAt(i));
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        return sb.toString();
    }
}
```


---

[Back to Home](../index.html)
[Next Lecture](./lecture7.html)