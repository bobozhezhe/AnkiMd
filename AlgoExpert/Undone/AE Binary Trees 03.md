# AlgoExpert

## Binary Trees 03 Invert Binary Tree

### Question

Write a function that takes in a Binary Tree and inverts it. In other words, the function should swap every left node in the tree for its corresponding right node.

Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree =    1
       /     \
      2       3
    /   \   /   \
   4     5 6     7
 /   \
8     9

### Sample Output

       1
    /     \
   3       2
 /   \   /   \
7     6 5     4
            /   \
           9     8

### Optimal Space & Time Complexity

O(n) time | O(d) space - where n is the number of nodes in the Binary Tree and d is the depth (height) of the Binary Tree

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space
  public static void invertBinaryTree(BinaryTree tree) {
    ArrayDeque<BinaryTree> queue = new ArrayDeque<BinaryTree>();
    queue.addLast(tree);
    while (queue.size() > 0) {
      BinaryTree current = queue.pollFirst();
      swapLeftAndRight(current);
      if (current.left != null) {
        queue.addLast(current.left);
      }
      if (current.right != null) {
        queue.addLast(current.right);
      }
    }
  }

  private static void swapLeftAndRight(BinaryTree tree) {
    BinaryTree left = tree.left;
    tree.left = tree.right;
    tree.right = left;
  }

  static class BinaryTree {
    public int value;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(d) space
  public static void invertBinaryTree(BinaryTree tree) {
    if (tree == null) {
      return;
    }
    swapLeftAndRight(tree);
    invertBinaryTree(tree.left);
    invertBinaryTree(tree.right);
  }

  private static void swapLeftAndRight(BinaryTree tree) {
    BinaryTree left = tree.left;
    tree.left = tree.right;
    tree.right = left;
  }

  static class BinaryTree {
    public int value;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
    }
  }
}

```
