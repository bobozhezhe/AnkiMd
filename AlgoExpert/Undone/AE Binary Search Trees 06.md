# AlgoExpert

## Binary Search Trees 06 Find Kth Largest Value In BST

### Question

Write a function that takes in a Binary Search Tree (BST) and a positive integer k and returns the kth largest integer contained in the BST.

You can assume that there will only be integer values in the BST and that k is less than or equal to the number of nodes in the tree.

Also, for the purpose of this question, duplicate integers will be treated as distinct values. In other words, the second largest value in a BST containing values {5, 7, 7} will be 7—not 5.

Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

### Sample Input

tree =   15
       /     \
      5      20
    /   \   /   \
   2     5 17   22
 /   \
1     3
k = 3

### Sample Output

17

### Optimal Space & Time Complexity

O(h + k) time | O(h) space - where h is the height of the tree and k is the input parameter

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

  // O(n) time | O(n) space - where n is the number of nodes in the tree
  public int findKthLargestValueInBst(BST tree, int k) {
    ArrayList<Integer> sortedNodeValues = new ArrayList<Integer>();
    inOrderTraverse(tree, sortedNodeValues);
    return sortedNodeValues.get(sortedNodeValues.size() - k);
  }

  public void inOrderTraverse(BST node, ArrayList<Integer> sortedNodeValues) {
    if (node == null) return;

    inOrderTraverse(node.left, sortedNodeValues);
    sortedNodeValues.add(node.value);
    inOrderTraverse(node.right, sortedNodeValues);
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
    public int numberOfNodesVisited;
    public int latestVisitedNodeValue;

    public TreeInfo(int numberOfNodesVisited, int latestVisitedNodeValue) {
      this.numberOfNodesVisited = numberOfNodesVisited;
      this.latestVisitedNodeValue = latestVisitedNodeValue;
    }
  }

  // O(h + k) time | O(h) space - where h is the height of the tree and k is the
  // input parameter
  public int findKthLargestValueInBst(BST tree, int k) {
    TreeInfo treeInfo = new TreeInfo(0, -1);
    reverseInOrderTraverse(tree, k, treeInfo);
    return treeInfo.latestVisitedNodeValue;
  }

  public void reverseInOrderTraverse(BST node, int k, TreeInfo treeInfo) {
    if (node == null || treeInfo.numberOfNodesVisited >= k) return;

    reverseInOrderTraverse(node.right, k, treeInfo);
    if (treeInfo.numberOfNodesVisited < k) {
      treeInfo.numberOfNodesVisited += 1;
      treeInfo.latestVisitedNodeValue = node.value;
      reverseInOrderTraverse(node.left, k, treeInfo);
    }
  }
}

```
