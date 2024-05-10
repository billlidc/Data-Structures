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


# 15. Binary Trees (Binary Search Trees)

## Overview

- Binary Trees combines the advantage of both **ordered arrays** and **LinkedLists**.

    1. Ordered Array
        - Using Binary Search, search in $O(\log n)$

    2. LinkedList
        - insert: $O(1)$
        - delete: $O(1)$


## Binary Trees Terminology

- **Root**: The node at the top of the tree.

- **Parent**: When any node (except the root) has exactly one edge running upward to another node. The node above is called parent of the node.

- **Child**: Any node may have one or more lines running downward to other nodes. These nodes below the given node called its children.

- **Leaf**: A node that has no children is called a leaf. There can be only one root in a tree but there can be many leaves.

- **Level/Height**: the level of a particular node refers to how many generations the node is from the root. The root is at level and its children are at level 1 and so on.
    - Count the number of edges
        - Empty binary tree has height of 0.
        - Binary Tree with only the root has height of 0.

- Balanced v.s. Unbalanced

- A binary tree has nodes that contain a data element, a *left* reference, and a *right* reference. In other words, every node in a binary tree can have at most two children.
- A **full** binary tree is a binary tree where each node has either zero or two children.
- A **complete** binary tree is a binary tree that is completely filled in reading (from left to right) each row with the possible exception of the bottom level.


## Binary Search Tree (BST)
The defining characteristic, or the **ordering invariant**, of binary search tree:

- At any node with a key value, k, in a binary search tree
    - all keys of the elements in the left sub-tree must be less than $k$
    - all keys of the elements in the right sub-tree must be greater than $k$
    - Meaning no duplicate keys are allowed


## BST Implementation

### BST Interface

```Java
public interface BSTInterface {

    /**
     * Searches for the specified key in the tree.
     * @param key key of the element to search
     * @return boolean value indication of success or failure
     */
    boolean find(int key);

    /**
     * Inserts a new element into the tree.
     * @param key key of the element
     * @param value value of the element
     */
    void insert(int key, double value);

    /**
     * Deletes an element from the tree using the specified key.
     * @param key key of the element to delete
     */
    void delete(int key);

    /**
     * Traverses and prints values of the tree in ascending order based on key.
     */
    void traverse();
}
```


```Java
public class BST implements BSTInterface {

    private Node root;
    
    public BST() {
        root = null;
    }

    /**
     * Searches for the specified key in the tree.
     * @param key key of the element to search
     * @return boolean value indication of success or failure
     */
    @Override
    public boolean find(int key) {
        // If empty tree
        if (root == null) {
            return false;
        }

        Node curr = root;
        
        while (curr.key != key) {
            if (curr.key < key) { // Go right if key is larger than current
                curr = curr.right;
            } else { // Go left if key is smaller than current
                curr = curr.left;
            }

            // If not found
            if (curr == null) { 
                return false;
            }
        }
        return true;
    }


    /**
     * Inserts a new element into the tree.
     * @param key key of the element
     * @param value value of the element
     */
    @Override
    public void insert(int key, double value) {
        Node newNode = new Node(key, value);
        // If empty tree
        if (root == null) {
            root = newNode;
            return;
        }

        Node parent = root;
        Node curr = root;

        while (true) {
            // Ignore duplicate keys
            if (curr.key == key) {
                return;
            }

            parent = curr; // update parent
            if (curr.key < key) {
                // Go right if key is larger than current
                curr = curr.right;
                if (curr == null) { // Found a spot
                    parent.right = newNode;
                    return;
                }
            } else {
                // Go left if key is smaller than current
                curr = curr.left;
                if (curr == null) { // Found a spot
                    parent.left = newNode;
                    return;
                }
            }
        }

    }



    /**
     * Deletes an element from the tree using the specified key.
     * @param key key of the element to delete
     */
    @Override
    public void delete(int key) {
        
        // If empty tree
        if (root == null) {
            return;
        }

        Node parent = root;
        Node curr = root;

        boolean isLeftChild = true;

        while (curr.key != key) {
            parent = curr;

            if (curr.key < key) { // Go right
                isLeftChild = false;
                curr = curr.right;
            } else { // Go left
                isLeftChild = true;
                curr = curr.left;
            }

            // Case 1: not found
            if (curr == null) {
                return;
            }
        }

        if (curr.left == null && curr.right == null) {
            // Case 2: a leaf
            
            if (curr == root) {
                root = null;
            } else if (isLeftChild) {
                parent.left = null;
            } else {
                parent.right = null;
            }

        } else if (curr.right == null) {
            // Case 3: one child (left child)

            if (curr == root) {
                root = curr.left;
            } else if (isLeftChild) {
                parent.left = curr.left;
            } else {
                parent.right = curr.left;
            }

        } else if (curr.left == null) {
            // Case 3: one child (right child)

            if (curr == root) {
                root = curr.right;
            } else if (isLeftChild) {
                parent.left = curr.right;
            } else {
                parent.right = curr.right;
            }

        } else {
            // Case 4: two children
            Node successor = getSuccessor(curr);

            if (curr == root) {
                root = successor;
            } else if (isLeftChild) {
                parent.left = successor;
            } else {
                parent.right = successor;
            }

            // Copy the left subtree
            successor.left = curr.left;
        }
    }

    /**
     * Helper method to find the successor of the toDelete node.
     * This tries to find the smallest value of the right subtree
     * of the toDelete node by going down to the left most node in the subtree
     * @param toDelete node to delete
     * @return the successor of the toDelete node
     */
    private Node getSuccessor(Node toDelete) {
        Node successorParent = toDelete;
        Node successor = toDelete;

        // Search from the root of the right subtree
        Node curr = toDelete.right;

        // Move down to left as far as possible
        while (curr != null) {
            successorParent = successor;
            successor = curr;
            curr = curr.left;
        }

        /*
         * Restructure the right subtree if successor is NOT the right child of toDelete
         */
        if (successor != toDelete.right) {
            successorParent.left = successor.right;
            successor.right = toDelete.right;
        }

        return successor;
    }


    /**
     * Traverses and prints values of the tree in ascending order based on key.
     */
    @Override
    public void traverse() {
        StringBuilder sb = new StringBuilder();
        inOrderHelper(root, sb);
        System.out.println(sb);
    }
    
    /**
     * Recursive helper method to traverse the tree.
     * @param toVisit node to visit
     * @param sb stringbuilder instance to append values of the node
     */
    private void inOrderHelper(Node toVisit, StringBuilder sb) {
        if (toVisit != null) {
            inOrderHelper(toVisit.left, sb);
            sb.append("[").append(toVisit.key).append(",").append(toVisit.value).append("]");
            inOrderHelper(toVisit.right, sb);
        }
    }


    /**
     * static nested Node class for Node.
     */
    private static class Node {
        private int key;
        private double value;
        private Node left;
        private Node right;

        Node(int k, double v) {
            key = k;
            value = v;
            left = null;
            right = null;
        }
    }
}
```


---

[Back to Home](../index.html)