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



# 12. Hash Table

## Hashing Steps

<div style="border: 1px solid black; padding: 10px; margin: 10px;">
  <p>
  Object -> int -> hashtable (array)

  1. Convert (Object -> int): `hashCode()`
  2. Compress (int -> hashtable (array)): `Modulus(%)`
  </p>
</div>




## Hash Table Operations
- `search(int key)`
- `delete(int key)`
- `insert(int key)`


## Hash Table Implementation

```Java
public class HashTable implements HashTableInterface {

    private static final DataItem DELETED = new DataItem(-1);
    
    private DataItem[] hashArray;
    
    public HashTable(int initialCapacity) {
        hashArray = new DataItem[initialCapacity];
    }

    /**
     * Returns true when the key is found.
     * @param key to search for
     * @return true if found, false if not found
     */
    @Override
    public boolean search(int key) {
        int hashVal = hashFunc(key);
        while (hashArray[hashVal] != null) {
            // If the key is found
            if (hashArray[hashVal].key == key) {
                return true;
            }
            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        return false;
    }

    /**
     * Deletes and returns the int key found in the table.
     * @param key to delete
     * @return deleted int from the table (if not found, -1)
     */
    @Override
    public int delete(int key) {
        int hashVal = hashFunc(key);

        while (hashArray[hashVal] != null) {    
            // If the key is found
            if (hashArray[hashVal].key == key) {
                DataItem item = hashArray[hashVal];
                hashArray[hashVal] = DELETED;
                return item.key;
            }

            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        return -1;
    }

    /**
     * Inserts a positive int key to the table.
     * @param key to insert (positive and unique)
     */
    @Override
    public void insert(int key) {
        int hashVal = hashFunc(key);

        // Search for an empty slot or a reusable slot, flagged as DELETED
        while ((hashArray[hashVal] != null) && (hashArray[hashVal] != DELETED)) {
            // Linear probing with step size of 1
            hashVal = hashVal + 1;
            hashVal = hashVal % hashArray.length;
        }
        hashArray[hashVal] = new DataItem(key);
    }

    /**
     * Rudimentary modular hashing function.
     * @param key for which hash value need to be calculated (only positive integers)
     * @return index/hash value of the specified key
     */
    private int hashFunc(int key) {
        return key % hashArray.length;
    }

    /**
     * Static nested class for DataItem.
     */
    private static class DataItem {
        private int key;
        DataItem(int k) {
            key = k;
        }
    }

}
```

## Aspects of Hash Functions

1. Deterministic

2. Randomness
    1. Quick computation - Machine-dependent
    2. Evenly distributed - Data-dependent


## Randomness

### 1. Random key values
```Java
index = key % hashArray.length;
```

### 2. Non-Random key values

Example: Car-part number

- Not guaranteed to be random between `0` to `999-999-99-99-99-9-999`
    ```
    033-400-03-94-05-0-535

    - Digits 0-2: Supplier number (1 to 999, currently up to 70)
    - Digits 3-5: Category code (100, 150, 200, 250, up to 850)
    - Digits 6-7: Month of introduction (1 to 12)
    - Digits 8-9: Year of introduction (00 to 99)
    - Digits 10-11: Serial number (1 to 99, never exceeds 100)
    - Digits 12: Toxic risk flag (0 or 1)
    - Digits 13-15: Checksum (sum of the other fields)
    ```


#### Generate a range of more random numbers
1. **Remove Non-Data**:
    - Squeeze key values
        - E.g. Category code should be squeezed to 0-15
    - Remove non-data values
        - E.g. Checksum is derived and does not incorporate any new information

2. **Use All Data**: Incorporate all available data, avoiding truncation of key values.

3. **Use Prime Number for Modulo Base**: Select a prime number as the modulo base (hash table length), which prevents clustering of keys that are multiples of the base, promoting a more uniform distribution of hash values.

4. **Use Folding**: Divide keys into groups of digits and summing up the groups, which enhances randomness and reduces collisions in the hash table.
    - Example: SSN `123-45-6789`
        - For table length $1009$:
            - Break into groups of three digits: $$123 + 456 + 789 = 1368 \% 1009 = 359$$
        - For table length $101$:
            - Break into groups of two digits: $$12 + 34 + 56 + 78 + 9 = 189 \% 101 = 88$$


---

[Back to Home](../index.html)
