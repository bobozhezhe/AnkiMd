# AlgoExpert

## Binary Trees 10 Flatten Binary Tree

### Question

Write a function that takes in a Binary Tree, flattens it, and returns its leftmost node.

A flattened Binary Tree is a structure that's nearly identical to a Doubly Linked List (except that nodes have left and right pointers instead of prev and next pointers), where nodes follow the original tree's left-to-right order.

Note that if the input Binary Tree happens to be a valid Binary Search Tree, the nodes in the flattened tree will be sorted.

The flattening should be done in place, meaning that the original data structure should be mutated (no new structure should be created).

Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree =      1
         /     \
        2       3
      /   \   /
     4     5 6
          / \
         7   8

### Sample Output

4 <-> 2 <-> 7 <-> 5 <-> 8 <-> 1 <-> 6 <-> 3 // the leftmost node with value 4

### Optimal Space & Time Complexity

O(n) time | O(d) space - where n is the number of nodes in the Binary Tree and d is the depth (height) of the Binary Tree

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space - where n is the number of nodes in the Binary Tree
  public static BinaryTree flattenBinaryTree(BinaryTree root) {
    List<BinaryTree> inOrderNodes = getNodesInOrder(root, new ArrayList<BinaryTree>());
    for (int i = 0; i < inOrderNodes.size() - 1; i++) {
      BinaryTree leftNode = inOrderNodes.get(i);
      BinaryTree rightNode = inOrderNodes.get(i + 1);
      leftNode.right = rightNode;
      rightNode.left = leftNode;
    }
    return inOrderNodes.get(0);
  }

  public static List<BinaryTree> getNodesInOrder(BinaryTree tree, List<BinaryTree> array) {
    if (tree != null) {
      getNodesInOrder(tree.left, array);
      array.add(tree);
      getNodesInOrder(tree.right, array);
    }
    return array;
  }

  static class BinaryTree {
    int value;
    BinaryTree left = null;
    BinaryTree right = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(d) space - where n is the number of nodes in the Binary Tree
  // and d is the depth (height) of the Binary Tree
  public static BinaryTree flattenBinaryTree(BinaryTree root) {
    BinaryTree leftMost = flattenTree(root)[0];
    return leftMost;
  }

  public static BinaryTree[] flattenTree(BinaryTree node) {
    BinaryTree leftMost;
    BinaryTree rightMost;

    if (node.left == null) {
      leftMost = node;
    } else {
      BinaryTree[] leftAndRightMostNodes = flattenTree(node.left);
      connectNodes(leftAndRightMostNodes[1], node);
      leftMost = leftAndRightMostNodes[0];
    }

    if (node.right == null) {
      rightMost = node;
    } else {
      BinaryTree[] leftAndRightMostNodes = flattenTree(node.right);
      connectNodes(node, leftAndRightMostNodes[0]);
      rightMost = leftAndRightMostNodes[1];
    }

    return new BinaryTree[] {leftMost, rightMost};
  }

  public static void connectNodes(BinaryTree left, BinaryTree right) {
    left.right = right;
    right.left = left;
  }

  static class BinaryTree {
    int value;
    BinaryTree left = null;
    BinaryTree right = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }
}

```
