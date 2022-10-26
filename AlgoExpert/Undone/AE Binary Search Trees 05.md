# AlgoExpert

## Binary Search Trees 05 Min Height BST

### Question

Write a function that takes in a non-empty sorted array of distinct integers, constructs a BST from the integers, and returns the root of the BST.

The function should minimize the height of the BST.

You've been provided with a BST class that you'll have to use to construct the BST.

Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

A BST is valid if and only if all of its nodes are valid BST nodes.

Note that the BST class already has an insert method which you can use if you want.

### Sample Input

array = [1, 2, 5, 7, 10, 13, 14, 15, 22]

### Sample Output

         10
       /     \
      2      14
    /   \   /   \
   1     5 13   15
          \       \
           7      22
// This is one example of a BST with min height
// that you could create from the input array.
// You could create other BSTs with min height
// from the same array; for example:
         10
       /     \
      5      15
    /   \   /   \
   2     7 13   22
 /           \
1            14

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the array

%

### Key Point

### Solution 1

```java
class Program {
  // O(nlog(n)) time | O(n) space - where n is the length of the array
  public static BST minHeightBst(List<Integer> array) {
    return constructMinHeightBst(array, null, 0, array.size() - 1);
  }

  public static BST constructMinHeightBst(List<Integer> array, BST bst, int startIdx, int endIdx) {
    if (endIdx < startIdx) return null;
    int midIdx = (startIdx + endIdx) / 2;
    int valueToAdd = array.get(midIdx);
    if (bst == null) {
      bst = new BST(valueToAdd);
    } else {
      bst.insert(valueToAdd);
    }
    constructMinHeightBst(array, bst, startIdx, midIdx - 1);
    constructMinHeightBst(array, bst, midIdx + 1, endIdx);
    return bst;
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
      left = null;
      right = null;
    }

    public void insert(int value) {
      if (value < this.value) {
        if (left == null) {
          left = new BST(value);
        } else {
          left.insert(value);
        }
      } else {
        if (right == null) {
          right = new BST(value);
        } else {
          right.insert(value);
        }
      }
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the array
  public static BST minHeightBst(List<Integer> array) {
    return constructMinHeightBst(array, null, 0, array.size() - 1);
  }

  public static BST constructMinHeightBst(List<Integer> array, BST bst, int startIdx, int endIdx) {
    if (endIdx < startIdx) return null;
    int midIdx = (startIdx + endIdx) / 2;
    BST newBstNode = new BST(array.get(midIdx));
    if (bst == null) {
      bst = newBstNode;
    } else {
      if (array.get(midIdx) < bst.value) {
        bst.left = newBstNode;
        bst = bst.left;
      } else {
        bst.right = newBstNode;
        bst = bst.right;
      }
    }
    constructMinHeightBst(array, bst, startIdx, midIdx - 1);
    constructMinHeightBst(array, bst, midIdx + 1, endIdx);
    return bst;
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
      left = null;
      right = null;
    }

    // We don't use this method for this solution.
    public void insert(int value) {
      if (value < this.value) {
        if (left == null) {
          left = new BST(value);
        } else {
          left.insert(value);
        }
      } else {
        if (right == null) {
          right = new BST(value);
        } else {
          right.insert(value);
        }
      }
    }
  }
}

```

### Solution 3

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the array
  public static BST minHeightBst(List<Integer> array) {
    return constructMinHeightBst(array, 0, array.size() - 1);
  }

  public static BST constructMinHeightBst(List<Integer> array, int startIdx, int endIdx) {
    if (endIdx < startIdx) return null;
    int midIdx = (startIdx + endIdx) / 2;
    BST bst = new BST(array.get(midIdx));
    bst.left = constructMinHeightBst(array, startIdx, midIdx - 1);
    bst.right = constructMinHeightBst(array, midIdx + 1, endIdx);
    return bst;
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
      left = null;
      right = null;
    }

    // We don't use this method for this solution.
    public void insert(int value) {
      if (value < this.value) {
        if (left == null) {
          left = new BST(value);
        } else {
          left.insert(value);
        }
      } else {
        if (right == null) {
          right = new BST(value);
        } else {
          right.insert(value);
        }
      }
    }
  }
}

```
