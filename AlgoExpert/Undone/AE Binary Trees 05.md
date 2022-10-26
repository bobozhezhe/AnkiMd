# AlgoExpert

## Binary Trees 05 Find Successor

### Question

Write a function that takes in a Binary Tree (where nodes have an additional pointer to their parent node) as well as a node contained in that tree and returns the given node's successor.

A node's successor is the next node to be visited (immediately after the given node) when traversing its tree using the in-order tree-traversal technique. A node has no successor if it's the last node to be visited in the in-order traversal.

If a node has no successor, your function should return None / null.

Each BinaryTree node has an integer value, a parent node, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree =
              1
            /   \
           2     3
         /   \
        4     5
       /
      6  
node = 5

### Sample Output

1
// This tree's in-order traversal order is:
// 6 -> 4 -> 2 -> 5 -> 1 -> 3
// 1 comes immediately after 5.

### Optimal Space & Time Complexity

O(h) time | O(1) space - where h is the height of the tree

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
    public BinaryTree parent = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  // O(n) time | O(n) space - where n is the number of nodes in the tree
  public BinaryTree findSuccessor(BinaryTree tree, BinaryTree node) {
    ArrayList<BinaryTree> inOrderTraversalOrder = new ArrayList<BinaryTree>();
    getInOrderTraversalOrder(tree, inOrderTraversalOrder);

    for (int i = 0; i < inOrderTraversalOrder.size(); i++) {
      BinaryTree currentNode = inOrderTraversalOrder.get(i);

      if (currentNode != node) {
        continue;
      }

      if (i == inOrderTraversalOrder.size() - 1) {
        return null;
      }
      return inOrderTraversalOrder.get(i + 1);
    }
    return null;
  }

  void getInOrderTraversalOrder(BinaryTree node, ArrayList<BinaryTree> order) {
    if (node == null) {
      return;
    }

    getInOrderTraversalOrder(node.left, order);
    order.add(node);
    getInOrderTraversalOrder(node.right, order);
  }
}

```

### Solution 2

```java
class Program {
  // This is an input class. Do not edit.
  static class BinaryTree {
    public int value;
    public BinaryTree left = null;
    public BinaryTree right = null;
    public BinaryTree parent = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  // O(h) time | O(1) space - where h is the height of the tree
  public BinaryTree findSuccessor(BinaryTree tree, BinaryTree node) {
    if (node.right != null) return getLeftmostChild(node.right);
    return getRightmostParent(node);
  }

  public BinaryTree getLeftmostChild(BinaryTree node) {
    BinaryTree currentNode = node;
    while (currentNode.left != null) {
      currentNode = currentNode.left;
    }

    return currentNode;
  }

  public BinaryTree getRightmostParent(BinaryTree node) {
    BinaryTree currentNode = node;
    while (currentNode.parent != null && currentNode.parent.right == currentNode) {
      currentNode = currentNode.parent;
    }

    return currentNode.parent;
  }
}

```
