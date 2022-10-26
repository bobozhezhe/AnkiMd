# AlgoExpert

## Binary Trees 13 Compare Leaf Traversal

### Question

Write a function that takes in the root nodes of two Binary Trees and returns a boolean representing whether their leaf traversals are the same.

The leaf traversal of a Binary Tree traverses only its leaf nodes from left to right. A leaf node is any node that has no left or right children.

For example, the leaf traversal of the following Binary Tree is 1, 3, 2.

   4
 /   \
1     5
    /   \
   3     2
Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree1 = 1
      /   \
     2     3
   /   \     \
  4     5     6
      /   \
     7     8
tree2 = 1
      /   \
     2     3
   /   \    \
  4     7    5
            /  \
           8    6

### Sample Output

true

### Optimal Space & Time Complexity

O(n + m) time | O(max(h1, h2)) space - where n is the number of nodes in the first Binary Tree, m is the number in the second, h1 is the height of the first Binary Tree, and h2 is the height of the second

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

  // O(n + m) time | O(h1 + h2) space - where n is the number of nodes in the first
  // Binary Tree, m is the number in the second, h1 is the height of the first Binary
  // Tree, and h2 is the height of the second
  public boolean compareLeafTraversal(BinaryTree tree1, BinaryTree tree2) {
    Stack<BinaryTree> tree1TraversalStack = new Stack<BinaryTree>();
    tree1TraversalStack.push(tree1);
    Stack<BinaryTree> tree2TraversalStack = new Stack<BinaryTree>();
    tree2TraversalStack.push(tree2);

    while (tree1TraversalStack.size() > 0 && tree2TraversalStack.size() > 0) {
      BinaryTree tree1Leaf = getNextLeafNode(tree1TraversalStack);
      BinaryTree tree2Leaf = getNextLeafNode(tree2TraversalStack);

      if (tree1Leaf.value != tree2Leaf.value) {
        return false;
      }
    }

    return (tree1TraversalStack.size() == 0) && (tree2TraversalStack.size() == 0);
  }

  public BinaryTree getNextLeafNode(Stack<BinaryTree> traversalStack) {
    BinaryTree currentNode = traversalStack.pop();

    while (!isLeafNode(currentNode)) {
      if (currentNode.right != null) {
        traversalStack.push(currentNode.right);
      }

      // We purposely add the left node to the stack after the
      // right node so that it gets popped off the stack first.
      if (currentNode.left != null) {
        traversalStack.push(currentNode.left);
      }

      currentNode = traversalStack.pop();
    }

    return currentNode;
  }

  public boolean isLeafNode(BinaryTree node) {
    return (node.left == null) && (node.right == null);
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

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  // O(n + m) time | O(max(h1, h2)) space - where n is the number of nodes in the first
  // Binary Tree, m is the number in the second, h1 is the height of the first Binary
  // Tree, and h2 is the height of the second
  public boolean compareLeafTraversal(BinaryTree tree1, BinaryTree tree2) {
    BinaryTree tree1LeafNodesLinkedList = connectLeafNodes(tree1, null, null)[0];
    BinaryTree tree2LeafNodesLinkedList = connectLeafNodes(tree2, null, null)[0];

    BinaryTree list1CurrentNode = tree1LeafNodesLinkedList;
    BinaryTree list2CurrentNode = tree2LeafNodesLinkedList;
    while (list1CurrentNode != null && list2CurrentNode != null) {
      if (list1CurrentNode.value != list2CurrentNode.value) return false;

      list1CurrentNode = list1CurrentNode.right;
      list2CurrentNode = list2CurrentNode.right;
    }

    return list1CurrentNode == null && list2CurrentNode == null;
  }

  BinaryTree[] connectLeafNodes(BinaryTree currentNode, BinaryTree head, BinaryTree previousNode) {
    if (currentNode == null) return new BinaryTree[] {head, previousNode};

    if (isLeafNode(currentNode)) {
      if (previousNode == null) {
        head = currentNode;
      } else {
        previousNode.right = currentNode;
      }

      previousNode = currentNode;
    }

    BinaryTree[] nodes = connectLeafNodes(currentNode.left, head, previousNode);
    BinaryTree leftHead = nodes[0];
    BinaryTree leftPreviousNode = nodes[1];

    return connectLeafNodes(currentNode.right, leftHead, leftPreviousNode);
  }

  public boolean isLeafNode(BinaryTree node) {
    return (node.left == null) && (node.right == null);
  }
}

```
