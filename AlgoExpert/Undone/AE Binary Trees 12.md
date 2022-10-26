# AlgoExpert

## Binary Trees 12 All Kinds Of Node Depths

### Question

The distance between a node in a Binary Tree and the tree's root is called the node's depth.

Write a function that takes in a Binary Tree and returns the sum of all of its subtrees' nodes' depths.

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

26
// The sum of the root tree's node depths is 16.
// The sum of the tree rooted at 2's node depths is 6.
// The sum of the tree rooted at 3's node depths is 2.
// The sum of the tree rooted at 4's node depths is 2.
// Summing all of these sums yields 26.

### Optimal Space & Time Complexity

Average case: when the tree is balanced O(n) time | O(h) space - where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree

%

### Key Point

### Solution 1

```java
class Program {
  // Average case: when the tree is balanced
  // O(nlog(n)) time | O(h) space - where n is the number of nodes in
  // the Binary Tree and h is the height of the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    int sumOfAllDepths = 0;
    List<BinaryTree> stack = new ArrayList<BinaryTree>();
    stack.add(root);
    while (stack.size() > 0) {
      BinaryTree node = stack.remove(stack.size() - 1);
      if (node == null) continue;
      sumOfAllDepths += nodeDepths(node, 0);
      stack.add(node.left);
      stack.add(node.right);
    }
    return sumOfAllDepths;
  }

  public static int nodeDepths(BinaryTree node, int depth) {
    if (node == null) return 0;
    return depth + nodeDepths(node.left, depth + 1) + nodeDepths(node.right, depth + 1);
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```

### Solution 2

```java
class Program {
  // Average case: when the tree is balanced
  // O(nlog(n)) time | O(h) space - where n is the number of nodes in
  // the Binary Tree and h is the height of the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    if (root == null) return 0;
    return allKindsOfNodeDepths(root.left) + allKindsOfNodeDepths(root.right) + nodeDepths(root, 0);
  }

  public static int nodeDepths(BinaryTree node, int depth) {
    if (node == null) return 0;
    return depth + nodeDepths(node.left, depth + 1) + nodeDepths(node.right, depth + 1);
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```

### Solution 3

```java
class Program {
  // Average case: when the tree is balanced
  // O(n) time | O(n) space - where n is the number of nodes in the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    Map<BinaryTree, Integer> nodeCounts = new HashMap<BinaryTree, Integer>();
    Map<BinaryTree, Integer> nodeDepths = new HashMap<BinaryTree, Integer>();
    addNodeCounts(root, nodeCounts);
    addNodeDepths(root, nodeDepths, nodeCounts);
    return sumAllNodeDepths(root, nodeDepths);
  }

  public static int sumAllNodeDepths(BinaryTree node, Map<BinaryTree, Integer> nodeDepths) {
    if (node == null) return 0;
    return sumAllNodeDepths(node.left, nodeDepths)
        + sumAllNodeDepths(node.right, nodeDepths)
        + nodeDepths.get(node);
  }

  public static void addNodeDepths(
      BinaryTree node, Map<BinaryTree, Integer> nodeDepths, Map<BinaryTree, Integer> nodeCounts) {
    nodeDepths.put(node, 0);
    if (node.left != null) {
      addNodeDepths(node.left, nodeDepths, nodeCounts);
      nodeDepths.put(
          node, nodeDepths.get(node) + nodeDepths.get(node.left) + nodeCounts.get(node.left));
    }
    if (node.right != null) {
      addNodeDepths(node.right, nodeDepths, nodeCounts);
      nodeDepths.put(
          node, nodeDepths.get(node) + nodeDepths.get(node.right) + nodeCounts.get(node.right));
    }
  }

  public static void addNodeCounts(BinaryTree node, Map<BinaryTree, Integer> nodeCounts) {
    nodeCounts.put(node, 1);
    if (node.left != null) {
      addNodeCounts(node.left, nodeCounts);
      nodeCounts.put(node, nodeCounts.get(node) + nodeCounts.get(node.left));
    }
    if (node.right != null) {
      addNodeCounts(node.right, nodeCounts);
      nodeCounts.put(node, nodeCounts.get(node) + nodeCounts.get(node.right));
    }
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```

### Solution 4

```java
class Program {
  // Average case: when the tree is balanced
  // O(n) time | O(h) space - where n is the number of nodes in
  // the Binary Tree and h is the height of the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    return getTreeInfo(root).sumOfAllDepths;
  }

  public static TreeInfo getTreeInfo(BinaryTree tree) {
    if (tree == null) {
      return new TreeInfo(0, 0, 0);
    }

    TreeInfo leftTreeInfo = getTreeInfo(tree.left);
    TreeInfo rightTreeInfo = getTreeInfo(tree.right);

    int sumOfLeftDepths = leftTreeInfo.sumOfDepths + leftTreeInfo.numNodesInTree;
    int sumOfRightDepths = rightTreeInfo.sumOfDepths + rightTreeInfo.numNodesInTree;

    int numNodesInTree = 1 + leftTreeInfo.numNodesInTree + rightTreeInfo.numNodesInTree;
    int sumOfDepths = sumOfLeftDepths + sumOfRightDepths;
    int sumOfAllDepths = sumOfDepths + leftTreeInfo.sumOfAllDepths + rightTreeInfo.sumOfAllDepths;

    return new TreeInfo(numNodesInTree, sumOfDepths, sumOfAllDepths);
  }

  static class TreeInfo {
    public int numNodesInTree;
    public int sumOfDepths;
    public int sumOfAllDepths;

    public TreeInfo(int numNodesInTree, int sumOfDepths, int sumOfAllDepths) {
      this.numNodesInTree = numNodesInTree;
      this.sumOfDepths = sumOfDepths;
      this.sumOfAllDepths = sumOfAllDepths;
    }
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```

### Solution 5

```java
class Program {
  // Average case: when the tree is balanced
  // O(n) time | O(h) space - where n is the number of nodes in
  // the Binary Tree and h is the height of the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    return allKindsOfNodeDepthsHelper(root, 0, 0);
  }

  public static int allKindsOfNodeDepthsHelper(BinaryTree node, int depthSum, int depth) {
    if (node == null) return 0;

    depthSum += depth;
    return depthSum
        + allKindsOfNodeDepthsHelper(node.left, depthSum, depth + 1)
        + allKindsOfNodeDepthsHelper(node.right, depthSum, depth + 1);
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```

### Solution 6

```java
class Program {
  // Average case: when the tree is balanced
  // O(n) time | O(h) space - where n is the number of nodes in
  // the Binary Tree and h is the height of the Binary Tree
  public static int allKindsOfNodeDepths(BinaryTree root) {
    return allKindsOfNodeDepthsHelper(root, 0);
  }

  public static int allKindsOfNodeDepthsHelper(BinaryTree node, int depth) {
    if (node == null) return 0;

    // Formula to calculate 1 + 2 + 3 + ... + depth - 1 + depth
    var depthSum = (depth * (depth + 1)) / 2;
    return depthSum
        + allKindsOfNodeDepthsHelper(node.left, depth + 1)
        + allKindsOfNodeDepthsHelper(node.right, depth + 1);
  }

  static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
      left = null;
      right = null;
    }
  }
}

```
