# AlgoExpert

## Binary Search Trees 01 Find Closest Value In BST

### Question

Write a function that takes in a Binary Search Tree (BST) and a target integer value and returns the closest value to that target value contained in the BST.

You can assume that there will only be one closest value.

Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

### Sample Input

tree =   10
       /     \
      5      15
    /   \   /   \
   2     5 13   22
 /           \
1            14
target = 12

### Sample Output

13

### Optimal Space & Time Complexity

Average: O(log(n)) time | O(1) space - where n is the number of nodes in the BST Worst: O(n) time | O(1) space - where n is the number of nodes in the BST

%

### Key Point

### Solution 1

```java
class Program {
  // Average: O(log(n)) time | O(log(n)) space
  // Worst: O(n) time | O(n) space
  public static int findClosestValueInBst(BST tree, int target) {
    return findClosestValueInBst(tree, target, tree.value);
  }

  public static int findClosestValueInBst(BST tree, int target, int closest) {
    if (Math.abs(target - closest) > Math.abs(target - tree.value)) {
      closest = tree.value;
    }
    if (target < tree.value && tree.left != null) {
      return findClosestValueInBst(tree.left, target, closest);
    } else if (target > tree.value && tree.right != null) {
      return findClosestValueInBst(tree.right, target, closest);
    } else {
      return closest;
    }
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
}

```

### Solution 2

```java
class Program {
  // Average: O(log(n)) time | O(1) space
  // Worst: O(n) time | O(1) space
  public static int findClosestValueInBst(BST tree, int target) {
    return findClosestValueInBst(tree, target, tree.value);
  }

  public static int findClosestValueInBst(BST tree, int target, int closest) {
    BST currentNode = tree;
    while (currentNode != null) {
      if (Math.abs(target - closest) > Math.abs(target - currentNode.value)) {
        closest = currentNode.value;
      }
      if (target < currentNode.value) {
        currentNode = currentNode.left;
      } else if (target > currentNode.value) {
        currentNode = currentNode.right;
      } else {
        break;
      }
    }
    return closest;
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
}

```
