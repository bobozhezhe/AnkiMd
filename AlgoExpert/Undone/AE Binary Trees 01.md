# AlgoExpert

## Binary Trees 01 Branch Sums

### Question

Write a function that takes in a Binary Tree and returns a list of its branch sums ordered from leftmost branch sum to rightmost branch sum.

A branch sum is the sum of all values in a Binary Tree branch. A Binary Tree branch is a path of nodes in a tree that starts at the root node and ends at any leaf node.

Each BinaryTree node has an integer value, a left child node, and a right child node. Children nodes can either be BinaryTree nodes themselves or None / null.

### Sample Input

tree =     1
        /     \
       2       3
     /   \    /  \
    4     5  6    7
  /   \  /
 8    9 10

### Sample Output

[15, 16, 18, 10, 11]
// 15 == 1 + 2 + 4 + 8
// 16 == 1 + 2 + 4 + 9
// 18 == 1 + 2 + 5 + 10
// 10 == 1 + 3 + 6
// 11 == 1 + 3 + 7

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the number of nodes in the Binary Tree

%

### Key Point

### Solution 1

```java
class Program {
  public static class BinaryTree {
    int value;
    BinaryTree left;
    BinaryTree right;

    BinaryTree(int value) {
      this.value = value;
      this.left = null;
      this.right = null;
    }
  }

  // O(n) time | O(n) space - where n is the number of nodes in the Binary Tree
  public static List<Integer> branchSums(BinaryTree root) {
    List<Integer> sums = new ArrayList<Integer>();
    calculateBranchSums(root, 0, sums);
    return sums;
  }

  public static void calculateBranchSums(BinaryTree node, int runningSum, List<Integer> sums) {
    if (node == null) return;

    int newRunningSum = runningSum + node.value;
    if (node.left == null && node.right == null) {
      sums.add(newRunningSum);
      return;
    }

    calculateBranchSums(node.left, newRunningSum, sums);
    calculateBranchSums(node.right, newRunningSum, sums);
  }
}

```
