# AlgoExpert

## Binary Search Trees 10 Right Smaller Than

### Question

Write a function that takes in an array of integers and returns an array of the same length, where each element in the output array corresponds to the number of integers in the input array that are to the right of the relevant index and that are strictly smaller than the integer at that index.

In other words, the value at output[i] represents the number of integers that are to the right of i and that are strictly smaller than input[i].

### Sample Input

array = [8, 5, 11, -1, 3, 4, 2]

### Sample Output

[5, 4, 4, 0, 1, 1, 0]
// There are 5 integers smaller than 8 to the right of it.
// There are 4 integers smaller than 5 to the right of it.
// There are 4 integers smaller than 11 to the right of it.
// Etc..

### Optimal Space & Time Complexity

Average case: when the created BST is balanced O(nlog(n)) time | O(n) space - where n is the length of the array --- Worst case: when the created BST is like a linked list O(n^2) time | O(n) space

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space - where n is the length of the array
  public static List<Integer> rightSmallerThan(List<Integer> array) {
    List<Integer> rightSmallerCounts = new ArrayList<Integer>();
    for (int i = 0; i < array.size(); i++) {
      int rightSmallerCount = 0;
      for (int j = i + 1; j < array.size(); j++) {
        if (array.get(j) < array.get(i)) {
          rightSmallerCount++;
        }
      }
      rightSmallerCounts.add(rightSmallerCount);
    }
    return rightSmallerCounts;
  }
}

```

### Solution 2

```java
class Program {
  // Average case: when the created BST is balanced
  // O(nlog(n)) time | O(n) space - where n is the length of the array
  // ---
  // Worst case: when the created BST is like a linked list
  // O(n^2) time | O(n) space
  public static List<Integer> rightSmallerThan(List<Integer> array) {
    if (array.size() == 0) return new ArrayList<Integer>();

    int lastIdx = array.size() - 1;
    SpecialBST bst = new SpecialBST(array.get(lastIdx), lastIdx, 0);
    for (int i = array.size() - 2; i >= 0; i--) {
      bst.insert(array.get(i), i);
    }

    List<Integer> rightSmallerCounts = new ArrayList<Integer>(array);
    getRightSmallerCounts(bst, rightSmallerCounts);
    return rightSmallerCounts;
  }

  public static void getRightSmallerCounts(SpecialBST bst, List<Integer> rightSmallerCounts) {
    if (bst == null) return;
    rightSmallerCounts.set(bst.idx, bst.numSmallerAtInsertTime);
    getRightSmallerCounts(bst.left, rightSmallerCounts);
    getRightSmallerCounts(bst.right, rightSmallerCounts);
  }

  static class SpecialBST {
    public int value;
    public int idx;
    public int numSmallerAtInsertTime;
    public int leftSubtreeSize;
    public SpecialBST left;
    public SpecialBST right;

    public SpecialBST(int value, int idx, int numSmallerAtInsertTime) {
      this.value = value;
      this.idx = idx;
      this.numSmallerAtInsertTime = numSmallerAtInsertTime;
      leftSubtreeSize = 0;
      left = null;
      right = null;
    }

    public void insert(int value, int idx) {
      insertHelper(value, idx, 0);
    }

    public void insertHelper(int value, int idx, int numSmallerAtInsertTime) {
      if (value < this.value) {
        leftSubtreeSize++;
        if (left == null) {
          left = new SpecialBST(value, idx, numSmallerAtInsertTime);
        } else {
          left.insertHelper(value, idx, numSmallerAtInsertTime);
        }
      } else {
        numSmallerAtInsertTime += leftSubtreeSize;
        if (value > this.value) numSmallerAtInsertTime++;
        if (right == null) {
          right = new SpecialBST(value, idx, numSmallerAtInsertTime);
        } else {
          right.insertHelper(value, idx, numSmallerAtInsertTime);
        }
      }
    }
  }
}

```

### Solution 3

```java
class Program {
  // Average case: when the created BST is balanced
  // O(nlog(n)) time | O(n) space - where n is the length of the array
  // ---
  // Worst case: when the created BST is like a linked list
  // O(n^2) time | O(n) space
  public static List<Integer> rightSmallerThan(List<Integer> array) {
    if (array.size() == 0) return new ArrayList<Integer>();

    List<Integer> rightSmallerCounts = new ArrayList<Integer>(array);
    int lastIdx = array.size() - 1;
    SpecialBST bst = new SpecialBST(array.get(lastIdx));
    rightSmallerCounts.set(lastIdx, 0);
    for (int i = array.size() - 2; i >= 0; i--) {
      bst.insert(array.get(i), i, rightSmallerCounts);
    }
    return rightSmallerCounts;
  }

  static class SpecialBST {
    public int value;
    public int leftSubtreeSize;
    public SpecialBST left;
    public SpecialBST right;

    public SpecialBST(int value) {
      this.value = value;
      leftSubtreeSize = 0;
      left = null;
      right = null;
    }

    public void insert(int value, int idx, List<Integer> rightSmallerCounts) {
      insertHelper(value, idx, rightSmallerCounts, 0);
    }

    public void insertHelper(
        int value, int idx, List<Integer> rightSmallerCounts, int numSmallerAtInsertTime) {
      if (value < this.value) {
        leftSubtreeSize++;
        if (left == null) {
          left = new SpecialBST(value);
          rightSmallerCounts.set(idx, numSmallerAtInsertTime);
        } else {
          left.insertHelper(value, idx, rightSmallerCounts, numSmallerAtInsertTime);
        }
      } else {
        numSmallerAtInsertTime += leftSubtreeSize;
        if (value > this.value) numSmallerAtInsertTime++;
        if (right == null) {
          right = new SpecialBST(value);
          rightSmallerCounts.set(idx, numSmallerAtInsertTime);
        } else {
          right.insertHelper(value, idx, rightSmallerCounts, numSmallerAtInsertTime);
        }
      }
    }
  }
}

```
