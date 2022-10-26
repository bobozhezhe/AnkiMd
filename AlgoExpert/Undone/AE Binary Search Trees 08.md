# AlgoExpert

## Binary Search Trees 08 Same BSTs

### Question

An array of integers is said to represent the Binary Search Tree (BST) obtained by inserting each integer in the array, from left to right, into the BST.

Write a function that takes in two arrays of integers and determines whether these arrays represent the same BST. Note that you're not allowed to construct any BSTs in your code.

A BST is a Binary Tree that consists only of BST nodes. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

### Sample Input

arrayOne = [10, 15, 8, 12, 94, 81, 5, 2, 11]
arrayTwo = [10, 8, 5, 15, 2, 12, 11, 94, 81]

### Sample Output

true // both arrays represent the BST below
         10
       /     \
      8      15
    /       /   \
   5      12    94
 /       /     /
2       11    81

### Optimal Space & Time Complexity

O(n^2) time | O(d) space - where n is the number of nodes in each array, respectively, and d is the depth of the BST that they represent

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n^2) space - where n is the number of
  // nodes in each array, respectively
  public static boolean sameBsts(List<Integer> arrayOne, List<Integer> arrayTwo) {
    if (arrayOne.size() != arrayTwo.size()) return false;

    if (arrayOne.size() == 0 && arrayTwo.size() == 0) return true;

    if (arrayOne.get(0).intValue() != arrayTwo.get(0).intValue()) return false;

    List<Integer> leftOne = getSmaller(arrayOne);
    List<Integer> leftTwo = getSmaller(arrayTwo);
    List<Integer> rightOne = getBiggerOrEqual(arrayOne);
    List<Integer> rightTwo = getBiggerOrEqual(arrayTwo);

    return sameBsts(leftOne, leftTwo) && sameBsts(rightOne, rightTwo);
  }

  public static List<Integer> getSmaller(List<Integer> array) {
    List<Integer> smaller = new ArrayList<Integer>();
    for (int i = 1; i < array.size(); i++) {
      if (array.get(i).intValue() < array.get(0).intValue()) smaller.add(array.get(i));
    }
    return smaller;
  }

  public static List<Integer> getBiggerOrEqual(List<Integer> array) {
    List<Integer> biggerOrEqual = new ArrayList<Integer>();
    for (int i = 1; i < array.size(); i++) {
      if (array.get(i).intValue() >= array.get(0).intValue()) biggerOrEqual.add(array.get(i));
    }
    return biggerOrEqual;
  }
}

```

### Solution 2

```java
class Program {
  // O(n^2) time | O(d) space - where n is the number of
  // nodes in each array, respectively, and d is the depth
  // of the BST that they represent
  public static boolean sameBsts(List<Integer> arrayOne, List<Integer> arrayTwo) {
    return areSameBsts(arrayOne, arrayTwo, 0, 0, Integer.MIN_VALUE, Integer.MAX_VALUE);
  }

  public static boolean areSameBsts(
      List<Integer> arrayOne,
      List<Integer> arrayTwo,
      int rootIdxOne,
      int rootIdxTwo,
      int minVal,
      int maxVal) {
    if (rootIdxOne == -1 || rootIdxTwo == -1) return rootIdxOne == rootIdxTwo;

    if (arrayOne.get(rootIdxOne).intValue() != arrayTwo.get(rootIdxTwo).intValue()) return false;

    int leftRootIdxOne = getIdxOfFirstSmaller(arrayOne, rootIdxOne, minVal);
    int leftRootIdxTwo = getIdxOfFirstSmaller(arrayTwo, rootIdxTwo, minVal);
    int rightRootIdxOne = getIdxOfFirstBiggerOrEqual(arrayOne, rootIdxOne, maxVal);
    int rightRootIdxTwo = getIdxOfFirstBiggerOrEqual(arrayTwo, rootIdxTwo, maxVal);

    int currentValue = arrayOne.get(rootIdxOne);
    boolean leftAreSame =
        areSameBsts(arrayOne, arrayTwo, leftRootIdxOne, leftRootIdxTwo, minVal, currentValue);
    boolean rightAreSame =
        areSameBsts(arrayOne, arrayTwo, rightRootIdxOne, rightRootIdxTwo, currentValue, maxVal);

    return leftAreSame && rightAreSame;
  }

  public static int getIdxOfFirstSmaller(List<Integer> array, int startingIdx, int minVal) {
    // Find the index of the first smaller value after the startingIdx.
    // Make sure that this value is greater than or equal to the minVal,
    // which is the value of the previous parent node in the BST. If it
    // isn't, then that value is located in the left subtree of the
    // previous parent node.
    for (int i = startingIdx + 1; i < array.size(); i++) {
      if (array.get(i).intValue() < array.get(startingIdx).intValue()
          && array.get(i).intValue() >= minVal) return i;
    }
    return -1;
  }

  public static int getIdxOfFirstBiggerOrEqual(List<Integer> array, int startingIdx, int maxVal) {
    // Find the index of the first bigger/equal value after the startingIdx.
    // Make sure that this value is smaller than maxVal, which is the value
    // of the previous parent node in the BST. If it isn't, then that value
    // is located in the right subtree of the previous parent node.
    for (int i = startingIdx + 1; i < array.size(); i++) {
      if (array.get(i).intValue() >= array.get(startingIdx).intValue()
          && array.get(i).intValue() < maxVal) return i;
    }
    return -1;
  }
}

```
