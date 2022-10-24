# AlgoExpert

## Dynamic Programming 05 Number Of Ways To Traverse Graph

### Question

You're given two positive integers representing the width and height of a grid-shaped, rectangular graph. Write a function that returns the number of ways to reach the bottom right corner of the graph when starting at the top left corner. Each move you take must either go down or right. In other words, you can never move up or left in the graph.

For example, given the graph illustrated below, with width = 2 and height = 3, there are three ways to reach the bottom right corner when starting at the top left corner:

 __
|_|_|
|_|_|
|_|_|
Down, Down, Right
Right, Down, Down
Down, Right, Down
Note: you may assume that width * height >= 2. In other words, the graph will never be a 1x1 grid.

### Sample Input

width = 4
height = 3

### Sample Output

10

### Optimal Space & Time Complexity

O(n + m) time | O(1) space - where n is the width of the graph and m is the height

%

### Key Point

### Solution 1

```java
class Program {

  // O(2^(n + m)) time | O(n + m) space - where n
  // is the width of the graph and m is the height
  public int numberOfWaysToTraverseGraph(int width, int height) {
    if (width == 1 || height == 1) {
      return 1;
    }

    return numberOfWaysToTraverseGraph(width - 1, height)
        + numberOfWaysToTraverseGraph(width, height - 1);
  }
}

```

### Solution 2

```java
class Program {

  // O(n * m) time | O(n * m) space - where n
  // is the width of the graph and m is the height
  public int numberOfWaysToTraverseGraph(int width, int height) {

    int[][] numberOfWays = new int[height + 1][width + 1];

    for (int widthIdx = 1; widthIdx < width + 1; widthIdx++) {
      for (int heightIdx = 1; heightIdx < height + 1; heightIdx++) {
        if (widthIdx == 1 || heightIdx == 1) {
          numberOfWays[heightIdx][widthIdx] = 1;
        } else {
          int waysLeft = numberOfWays[heightIdx][widthIdx - 1];
          int waysUp = numberOfWays[heightIdx - 1][widthIdx];
          numberOfWays[heightIdx][widthIdx] = waysLeft + waysUp;
        }
      }
    }

    return numberOfWays[height][width];
  }
}

```

### Solution 3

```java
class Program {

  // O(n + m) time | O(1) space - where n is
  // the width of the graph and m is the height
  public int numberOfWaysToTraverseGraph(int width, int height) {
    int xDistanceToCorner = width - 1;
    int yDistanceToCorner = height - 1;

    // The number of permutations of right and down movements
    // is the number of ways to reach the bottom right corner.
    int numerator = factorial(xDistanceToCorner + yDistanceToCorner);
    int denominator = factorial(xDistanceToCorner) * factorial(yDistanceToCorner);
    return numerator / denominator;
  }

  public int factorial(int num) {
    int result = 1;

    for (int n = 2; n < num + 1; n++) {
      result *= n;
    }

    return result;
  }
}

```
