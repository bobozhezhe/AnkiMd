# AlgoExpert

## Recursion 12 Number Of Binary Tree Topologies

### Question

Write a function that takes in a non-negative integer n and returns the number of possible Binary Tree topologies that can be created with exactly n nodes.

A Binary Tree topology is defined as any Binary Tree configuration, irrespective of node values. For instance, there exist only two Binary Tree topologies when n is equal to 2: a root node with a left node, and a root node with a right node.

Note that when n is equal to 0, there's one topology that can be created: the None / null node.

### Sample Input

n = 3

### Sample Output

5

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the input number

%

### Key Point

### Solution 1

```java
class Program {
  // Upper Bound: O((n*(2n)!)/(n!(n+1)!)) time | O(n) space
  public static int numberOfBinaryTreeTopologies(int n) {
    if (n == 0) {
      return 1;
    }
    int numberOfTrees = 0;
    for (int leftTreeSize = 0; leftTreeSize < n; leftTreeSize++) {
      int rightTreeSize = n - 1 - leftTreeSize;
      int numberOfLeftTrees = numberOfBinaryTreeTopologies(leftTreeSize);
      int numberOfRightTrees = numberOfBinaryTreeTopologies(rightTreeSize);
      numberOfTrees += numberOfLeftTrees * numberOfRightTrees;
    }
    return numberOfTrees;
  }
}

```

### Solution 2

```java
class Program {
  // O(n^2) time | O(n) space
  public static int numberOfBinaryTreeTopologies(int n) {
    Map<Integer, Integer> cache = new HashMap<Integer, Integer>();
    cache.put(0, 1);
    return numberOfBinaryTreeTopologies(n, cache);
  }

  public static int numberOfBinaryTreeTopologies(int n, Map<Integer, Integer> cache) {
    if (cache.containsKey(n)) {
      return cache.get(n);
    }
    int numberOfTrees = 0;
    for (int leftTreeSize = 0; leftTreeSize < n; leftTreeSize++) {
      int rightTreeSize = n - 1 - leftTreeSize;
      int numberOfLeftTrees = numberOfBinaryTreeTopologies(leftTreeSize, cache);
      int numberOfRightTrees = numberOfBinaryTreeTopologies(rightTreeSize, cache);
      numberOfTrees += numberOfLeftTrees * numberOfRightTrees;
    }
    cache.put(n, numberOfTrees);
    return numberOfTrees;
  }
}

```

### Solution 3

```java
class Program {
  // O(n^2) time | O(n) space
  public static int numberOfBinaryTreeTopologies(int n) {
    List<Integer> cache = new ArrayList<Integer>();
    cache.add(1);
    for (int m = 1; m < n + 1; m++) {
      int numberOfTrees = 0;
      for (int leftTreeSize = 0; leftTreeSize < m; leftTreeSize++) {
        int rightTreeSize = m - 1 - leftTreeSize;
        int numberOfLeftTrees = cache.get(leftTreeSize);
        int numberOfRightTrees = cache.get(rightTreeSize);
        numberOfTrees += numberOfLeftTrees * numberOfRightTrees;
      }
      cache.add(numberOfTrees);
    }
    return cache.get(n);
  }
}

```
