# AlgoExpert

## Binary Trees 06 Height Balanced Binary Tree

### Question

You're given the root node of a Binary Tree. Write a function that returns true if this Binary Tree is height balanced and false if it isn't.

A Binary Tree is height balanced if for each node in the tree, the difference between the height of its left subtree and the height of its right subtree is at most 1.

Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree = 1
     /   \
    2     3
  /   \     \
 4     5     6
     /   \
    7     8

### Sample Output

true

### Optimal Space & Time Complexity

O(n) time | O(h) space - where n is the number of nodes in the binary tree

%

### Key Point

### Solution 1

```java
class Program {
  // This is an input class. Do not edit.
  static class BinaryTree {
    public int value;
    public BinaryTree left = null;
    public BinaryTree right = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  static class TreeInfo {
    public boolean isBalanced;
    public int height;

    public TreeInfo(boolean isBalanced, int height) {
      this.isBalanced = isBalanced;
      this.height = height;
    }
  }

  // O(n) time | O(h) space - where n is the number of nodes in the binary tree
  public boolean heightBalancedBinaryTree(BinaryTree tree) {
    TreeInfo treeInfo = getTreeInfo(tree);
    return treeInfo.isBalanced;
  }

  public TreeInfo getTreeInfo(BinaryTree node) {
    if (node == null) {
      return new TreeInfo(true, -1);
    }

    TreeInfo leftSubtreeInfo = getTreeInfo(node.left);
    TreeInfo rightSubtreeInfo = getTreeInfo(node.right);

    boolean isBalanced =
        leftSubtreeInfo.isBalanced
            && rightSubtreeInfo.isBalanced
            && Math.abs(leftSubtreeInfo.height - rightSubtreeInfo.height) <= 1;

    int height = Math.max(leftSubtreeInfo.height, rightSubtreeInfo.height) + 1;
    return new TreeInfo(isBalanced, height);
  }
}

```
