# AlgoExpert

## Binary Trees 08 Find Nodes Distance K

### Question

You're given the root node of a Binary Tree, a target value of a node that's contained in the tree, and a positive integer k. Write a function that returns the values of all the nodes that are exactly distance k from the node with target value.

The distance between two nodes is defined as the number of edges that must be traversed to go from one node to the other. For example, the distance between a node and its immediate left or right child is 1. The same holds in reverse: the distance between a node and its parent is 1. In a tree of three nodes where the root node has a left and right child, the left and right children are distance 2 from each other.

Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

Note that all BinaryTree node values will be unique, and your function can return the output values in any order.

### Sample Input

tree = 1
     /   \
    2     3
  /   \     \
 4     5     6
           /   \
          7     8
target = 3
k = 2

### Sample Output

[2, 7, 8] // These values could be ordered differently.

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the number of nodes in the tree

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

  static class Pair<U, V> {
    public final U first;
    public final V second;

    private Pair(U first, V second) {
      this.first = first;
      this.second = second;
    }
  }

  // O(n) time | O(n) space - where n is the number of nodes in the tree
  public ArrayList<Integer> findNodesDistanceK(BinaryTree tree, int target, int k) {
    HashMap<Integer, BinaryTree> nodesToParents = new HashMap<Integer, BinaryTree>();
    populateNodesToParents(tree, nodesToParents, null);
    BinaryTree targetNode = getNodeFromValue(target, tree, nodesToParents);
    return breadthFirstSearchForNodesDistanceK(targetNode, nodesToParents, k);
  }

  public ArrayList<Integer> breadthFirstSearchForNodesDistanceK(
      BinaryTree targetNode, HashMap<Integer, BinaryTree> nodesToParents, int k) {
    Queue<Pair<BinaryTree, Integer>> queue = new LinkedList<Pair<BinaryTree, Integer>>();
    queue.offer(new Pair<BinaryTree, Integer>(targetNode, 0));

    HashSet<Integer> seen = new HashSet<Integer>(targetNode.value);
    seen.add(targetNode.value);

    while (queue.size() > 0) {
      Pair<BinaryTree, Integer> vals = queue.poll();
      BinaryTree currentNode = vals.first;
      int distanceFromTarget = vals.second;

      if (distanceFromTarget == k) {
        ArrayList<Integer> nodeDistanceK = new ArrayList<Integer>();
        for (Pair<BinaryTree, Integer> pair : queue) {
          nodeDistanceK.add(pair.first.value);
        }
        nodeDistanceK.add(currentNode.value);
        return nodeDistanceK;
      }

      List<BinaryTree> connectedNodes = new ArrayList<BinaryTree>();
      connectedNodes.add(currentNode.left);
      connectedNodes.add(currentNode.right);
      connectedNodes.add(nodesToParents.get(currentNode.value));

      for (BinaryTree node : connectedNodes) {
        if (node == null) continue;

        if (seen.contains(node.value)) continue;

        seen.add(node.value);
        queue.add(new Pair<BinaryTree, Integer>(node, distanceFromTarget + 1));
      }
    }

    return new ArrayList<Integer>();
  }

  public BinaryTree getNodeFromValue(
      int value, BinaryTree tree, HashMap<Integer, BinaryTree> nodesToParents) {
    if (tree.value == value) return tree;

    BinaryTree nodeParent = nodesToParents.get(value);
    if (nodeParent.left != null && nodeParent.left.value == value) return nodeParent.left;

    return nodeParent.right;
  }

  public void populateNodesToParents(
      BinaryTree node, Map<Integer, BinaryTree> nodesToParents, BinaryTree parent) {
    if (node != null) {
      nodesToParents.put(node.value, parent);
      populateNodesToParents(node.left, nodesToParents, node);
      populateNodesToParents(node.right, nodesToParents, node);
    }
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

  // O(n) time | O(n) space - where n is the number of nodes in the tree
  public ArrayList<Integer> findNodesDistanceK(BinaryTree tree, int target, int k) {
    ArrayList<Integer> nodesDistanceK = new ArrayList<Integer>();
    findDistanceFromNodeToTarget(tree, target, k, nodesDistanceK);
    return nodesDistanceK;
  }

  public int findDistanceFromNodeToTarget(
      BinaryTree node, int target, int k, ArrayList<Integer> nodesDistanceK) {
    if (node == null) return -1;

    if (node.value == target) {
      addSubtreeNodeAtDistanceK(node, 0, k, nodesDistanceK);
      return 1;
    }

    int leftDistance = findDistanceFromNodeToTarget(node.left, target, k, nodesDistanceK);
    int rightDistance = findDistanceFromNodeToTarget(node.right, target, k, nodesDistanceK);

    if (leftDistance == k || rightDistance == k) nodesDistanceK.add(node.value);

    if (leftDistance != -1) {
      addSubtreeNodeAtDistanceK(node.right, leftDistance + 1, k, nodesDistanceK);
      return leftDistance + 1;
    }

    if (rightDistance != -1) {
      addSubtreeNodeAtDistanceK(node.left, rightDistance + 1, k, nodesDistanceK);
      return rightDistance + 1;
    }

    return -1;
  }

  void addSubtreeNodeAtDistanceK(
      BinaryTree node, int distance, int k, ArrayList<Integer> nodesDistanceK) {
    if (node == null) return;

    if (distance == k) {
      nodesDistanceK.add(node.value);
    } else {
      addSubtreeNodeAtDistanceK(node.left, distance + 1, k, nodesDistanceK);
      addSubtreeNodeAtDistanceK(node.right, distance + 1, k, nodesDistanceK);
    }
  }
}

```
