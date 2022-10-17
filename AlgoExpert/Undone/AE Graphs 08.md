# AlgoExpert

## Graphs 08 Minimum Passes Of Matrix

### Question

Write a function that takes in an integer matrix of potentially unequal height and width and returns the minimum number of passes required to convert all negative integers in the matrix to positive integers.

A negative integer in the matrix can only be converted to a positive integer if one or more of its adjacent elements is positive. An adjacent element is an element that is to the left, to the right, above, or below the current element in the matrix. Converting a negative to a positive simply involves multiplying it by -1.

Note that the 0 value is neither positive nor negative, meaning that a 0 can't convert an adjacent negative to a positive.

A single pass through the matrix involves converting all the negative integers that can be converted at a particular point in time. For example, consider the following input matrix:

[
  [0, -2, -1],
  [-5, 2, 0],
  [-6, -2, 0],
]
After a first pass, only 3 values can be converted to positives:

[
  [0, 2, -1],
  [5, 2, 0],
  [-6, 2, 0],
]
After a second pass, the remaining negative values can all be converted to positives:

[
  [0, 2, 1],
  [5, 2, 0],
  [6, 2, 0],
]
Note that the input matrix will always contain at least one element. If the negative integers in the input matrix can't all be converted to positives, regardless of how many passes are run, your function should return -1.

### Sample Input

matrix = [
  [0, -1, -3, 2, 0],
  [1, -2, -5, -1, -3],
  [3, 0, 0, -4, -1],
]

### Sample Output

3

### Optimal Space & Time Complexity

`O(w *h) time | O(w* h)` space - where w is the width of the matrix and h is the height

%

### Key Point

### Solution 1

```java
class Program {
  // O(w * h) time | O(w * h) space - where w is the
  // width of the matrix and h is the height
  public int minimumPassesOfMatrix(int[][] matrix) {
    int passes = convertNegatives(matrix);
    return (!containsNegative(matrix)) ? passes - 1 : -1;
  }

  public int convertNegatives(int[][] matrix) {

    ArrayList<int[]> nextPassQueue = getAllPositivePositions(matrix);

    int passes = 0;

    while (nextPassQueue.size() > 0) {
      ArrayList<int[]> currentPassQueue = nextPassQueue;
      nextPassQueue = new ArrayList<int[]>();

      while (currentPassQueue.size() > 0) {
        // In Java, removing elements from the start of a list is an O(n)-time operation.
        // To make this an O(1)-time operation, we could use a more legitimate queue structure.
        // For our time complexity analysis, we'll assume this runs in O(1) time.
        // Also, for this particular solution (Solution #1), we could actually
        // just turn this queue into a stack and replace `.remove(0)` with the
        // constant-time `pop()` operation.
        int[] vals = currentPassQueue.remove(0);
        int currentRow = vals[0];
        int currentCol = vals[1];

        ArrayList<int[]> adjacentPositions = getAdjacentPositions(currentRow, currentCol, matrix);
        for (int[] position : adjacentPositions) {
          int row = position[0];
          int col = position[1];

          int value = matrix[row][col];
          if (value < 0) {
            matrix[row][col] *= -1;
            nextPassQueue.add(new int[] {row, col});
          }
        }
      }

      passes += 1;
    }

    return passes;
  }

  public ArrayList<int[]> getAllPositivePositions(int[][] matrix) {
    ArrayList<int[]> positivePositions = new ArrayList<int[]>();

    for (int row = 0; row < matrix.length; row++) {
      for (int col = 0; col < matrix[row].length; col++) {
        int value = matrix[row][col];
        if (value > 0) {
          positivePositions.add(new int[] {row, col});
        }
      }
    }

    return positivePositions;
  }

  public ArrayList<int[]> getAdjacentPositions(int row, int col, int[][] matrix) {
    ArrayList<int[]> adjacentPositions = new ArrayList<int[]>();

    if (row > 0) {
      adjacentPositions.add(new int[] {row - 1, col});
    }
    if (row < matrix.length - 1) {
      adjacentPositions.add(new int[] {row + 1, col});
    }
    if (col > 0) {
      adjacentPositions.add(new int[] {row, col - 1});
    }
    if (col < (matrix[0].length - 1)) {
      adjacentPositions.add(new int[] {row, col + 1});
    }

    return adjacentPositions;
  }

  public boolean containsNegative(int[][] matrix) {
    for (int[] row : matrix) {
      for (int value : row) {
        if (value < 0) {
          return true;
        }
      }
    }

    return false;
  }
}

```

### Solution 2

```java
class Program {
  // O(w * h) time | O(w * h) space - where w is the
  // width of the matrix and h is the height
  public int minimumPassesOfMatrix(int[][] matrix) {
    int passes = convertNegatives(matrix);
    return (!containsNegative(matrix)) ? passes - 1 : -1;
  }


  public int convertNegatives(int[][] matrix) {

    ArrayList<int[]> queue = getAllPositivePositions(matrix);

    int passes = 0;

    while (queue.size() > 0) {
      int currentSize = queue.size();

      while (currentSize > 0) {
        // In Java, removing elements from the start of a list is an O(n)-time operation.
        // To make this an O(1)-time operation, we could use a more legitimate queue structure.
        // For our time complexity analysis, we'll assume this runs in O(1) time.
        int[] vals = queue.remove(0);
        int currentRow = vals[0];
        int currentCol = vals[1];

        ArrayList<int[]> adjacentPositions = getAdjacentPositions(currentRow, currentCol, matrix);
        for (int[] position : adjacentPositions) {
          int row = position[0];
          int col = position[1];

          int value = matrix[row][col];
          if (value < 0) {
            matrix[row][col] *= -1;
            queue.add(new int[] {row, col});
          }
        }

        currentSize -= 1;
      }

      passes += 1;
    }

    return passes;
  }

  public ArrayList<int[]> getAllPositivePositions(int[][] matrix) {
    ArrayList<int[]> positivePositions = new ArrayList<int[]>();

    for (int row = 0; row < matrix.length; row++) {
      for (int col = 0; col < matrix[row].length; col++) {
        int value = matrix[row][col];
        if (value > 0) {
          positivePositions.add(new int[] {row, col});
        }
      }
    }

    return positivePositions;
  }

  public ArrayList<int[]> getAdjacentPositions(int row, int col, int[][] matrix) {
    ArrayList<int[]> adjacentPositions = new ArrayList<int[]>();

    if (row > 0) {
      adjacentPositions.add(new int[] {row - 1, col});
    }
    if (row < matrix.length - 1) {
      adjacentPositions.add(new int[] {row + 1, col});
    }
    if (col > 0) {
      adjacentPositions.add(new int[] {row, col - 1});
    }
    if (col < (matrix[0].length - 1)) {
      adjacentPositions.add(new int[] {row, col + 1});
    }

    return adjacentPositions;
  }

  public boolean containsNegative(int[][] matrix) {
    for (int[] row : matrix) {
      for (int value : row) {
        if (value < 0) {
          return true;
        }
      }
    }

    return false;
  }
}

```
