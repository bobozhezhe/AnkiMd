# AlgoExpert

## LinkedLists 09 LRU Cache

### Question

Implement an LRUCache class for a Least Recently Used (LRU) cache. The class should support:

Inserting key-value pairs with the insertKeyValuePair method.
Retrieving a key's value with the getValueFromKey method.
Retrieving the most recently used (the most recently inserted or retrieved) key with the getMostRecentKey method.
Each of these methods should run in constant time.

Additionally, the LRUCache class should store a maxSize property set to the size of the cache, which is passed in as an argument during instantiation. This size represents the maximum number of key-value pairs that the cache can store at once. If a key-value pair is inserted in the cache when it has reached maximum capacity, the least recently used key-value pair should be evicted from the cache and no longer retrievable; the newly added key-value pair should effectively replace it.

Note that inserting a key-value pair with an already existing key should simply replace the key's value in the cache with the new value and shouldn't evict a key-value pair if the cache is full. Lastly, attempting to retrieve a value from a key that isn't in the cache should return None / null.

### Sample Usage

// All operations below are performed sequentially.
LRUCache(3): - // instantiate an LRUCache of size 3
insertKeyValuePair("b", 2): -
insertKeyValuePair("a", 1): -
insertKeyValuePair("c", 3): -
getMostRecentKey(): "c" // "c" was the most recently inserted key
getValueFromKey("a"): 1
getMostRecentKey(): "a" // "a" was the most recently retrieved key
insertKeyValuePair("d", 4): - // the cache had 3 entries; the least recently used one is evicted
getValueFromKey("b"): None // "b" was evicted in the previous operation
insertKeyValuePair("a", 5): - // "a" already exists in the cache so its value just gets replaced
getValueFromKey("a"): 5

### Optimal Space & Time Complexity

(all 3 methods) O(1) time | O(1) space

%

### Key Point

### Solution 1

```java
class Program {
      } else if (head == null) {
        head = node;
        tail = node;
      } else if (head == tail) {
        tail.prev = node;
        head = node;
        head.next = tail;
      } else {
        if (tail == node) {
          removeTail();
        }
        node.removeBindings();
        head.prev = node;
        node.next = head;
        head = node;
      }
    }

    public void removeTail() {
      if (tail == null) {
        return;
      }
      if (tail == head) {
        head = null;
        tail = null;
        return;
      }
      tail = tail.prev;
      tail.next = null;
    }
  }

  static class DoublyLinkedListNode {
    String key;
    int value;
    DoublyLinkedListNode prev = null;
    DoublyLinkedListNode next = null;

    public DoublyLinkedListNode(String key, int value) {
      this.key = key;
      this.value = value;
    }

    public void removeBindings() {
      if (prev != null) {
        prev.next = next;
      }
      if (next != null) {
        next.prev = prev;
      }
      prev = null;
      next = null;
    }
  }

  static class LRUResult {
    boolean found;
    int value;

    public LRUResult(boolean found, int value) {
      this.found = found;
      this.value = value;
    }
  }
}

```
