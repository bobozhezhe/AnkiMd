# AlgoExpert

## Binary Search Trees 07 Reconstruct BST

### Question

The pre-order traversal of a Binary Tree is a traversal technique that starts at the tree's root node and visits nodes in the following order:

Current node
Left subtree
Right subtree
Given a non-empty array of integers representing the pre-order traversal of a Binary Search Tree (BST), write a function that creates the relevant BST and returns its root node.

The input array will contain the values of BST nodes in the order in which these nodes would be visited with a pre-order traversal.

Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

### Sample Input

preOrderTraversalValues = [10, 4, 2, 1, 5, 17, 19, 18]

### Sample Output

        10 
      /    \
     4      17
   /   \      \
  2     5     19
 /           /
1           18

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {

  // This is an input class. Do not edit.
  static class BST {
    public int value;
    public BST left = null;
    public BST right = null;

    public BST(int value) {
      this.value = value;
    }
  }

  // O(n^2) time | O(n) space - where n is the length of the input array
  public BST reconstructBst(List<Integer> preOrderTraversalValues) {

    if (preOrderTraversalValues.size() == 0) {
      return null;
    }

    int currentValue = preOrderTraversalValues.get(0);
    int rightSubtreeRootIdx = preOrderTraversalValues.size();

    for (int idx = 1; idx < preOrderTraversalValues.size(); idx++) {
      int value = preOrderTraversalValues.get(idx);
      if (value >= currentValue) {
        rightSubtreeRootIdx = idx;
        break;
      }
    }

    BST leftSubtree = reconstructBst(preOrderTraversalValues.subList(1, rightSubtreeRootIdx));
    BST rightSubtree =
        reconstructBst(
            preOrderTraversalValues.subList(rightSubtreeRootIdx, preOrderTraversalValues.size()));

    BST bst = new BST(currentValue);
    bst.left = leftSubtree;
    bst.right = rightSubtree;

    return bst;
  }
}

```

### Solution 2

```java
class Program {

  // This is an input class. Do not edit.
  static class BST {
    public int value;
    public BST left = null;
    public BST right = null;

    public BST(int value) {
      this.value = value;
    }
  }

  static class TreeInfo {
    public int rootIdx;

    public TreeInfo(int rootIdx) {
      this.rootIdx = rootIdx;
    }
  }

  // O(n) time | O(n) space - where n is the length of the input array
  public BST reconstructBst(List<Integer> preOrderTraversalValues) {
    TreeInfo treeInfo = new TreeInfo(0);
    return reconstructBstFromRange(
        Integer.MIN_VALUE, Integer.MAX_VALUE, preOrderTraversalValues, treeInfo);
  }

  public BST reconstructBstFromRange(
      int lowerBound,
      int upperBound,
      List<Integer> preOrderTraversalValues,
      TreeInfo currentSubtreeInfo) {
    if (currentSubtreeInfo.rootIdx == preOrderTraversalValues.size()) {
      return null;
    }

    int rootValue = preOrderTraversalValues.get(currentSubtreeInfo.rootIdx);
    if (rootValue < lowerBound || rootValue >= upperBound) {
      return null;
    }

    currentSubtreeInfo.rootIdx += 1;
    BST leftSubtree =
        reconstructBstFromRange(lowerBound, rootValue, preOrderTraversalValues, currentSubtreeInfo);
    BST rightSubtree =
        reconstructBstFromRange(rootValue, upperBound, preOrderTraversalValues, currentSubtreeInfo);

    BST bst = new BST(rootValue);
    bst.left = leftSubtree;
    bst.right = rightSubtree;
    return bst;
  }
}

```
