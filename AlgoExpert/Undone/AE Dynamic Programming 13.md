# AlgoExpert

## Dynamic Programming 13 Maximum Sum Submatrix

### Question

You're given a two-dimensional array (a matrix) of potentially unequal height and width that's filled with integers. You're also given a positive integer size. Write a function that returns the maximum sum that can be generated from a submatrix with dimensions size * size.

For example, consider the following matrix:

[
  [2, 4],
  [5, 6],
  [-3, 2],
]
If size = 2, then the 2x2 submatrices to consider are:

```c
[2, 4]
[5, 6]
------

[5, 6]
[-3, 2]
```

The sum of the elements in the first submatrix is 17, and the sum of the elements in the second submatrix is 10. In this example, your function should return 17.

Note: size will always be at least 1, and the dimensions of the input matrix will always be at least size * size.

### Sample Input

matrix =
[
  [5, 3, -1, 5],
  [-7, 3, 7, 4],
  [12, 8, 0, 0],
  [1, -8, -8, 2],
]
size = 2

### Sample Output

18
// [
//   [., ., ., .],
//   [., 3, 7, .],
//   [., 8, 0, .],
//   [., ., ., .],
// ]

### Optimal Space & Time Complexity

O(w *h) time | O(w* h) space - where w is the width of the matrix and h is the height

%

### Key Point

### Solution 1

```java
class Program {

  // O(w * h) time | O(w * h) space - where w is
  // the width of the matrix and h is the height
  public int maximumSumSubmatrix(int[][] matrix, int size) {
    int[][] sums = createSumMatrix(matrix);
    int maxSubMatrixSum = Integer.MIN_VALUE;

    for (int row = size - 1; row < matrix.length; row++) {
      for (int col = size - 1; col < matrix[row].length; col++) {
        int total = sums[row][col];

        boolean touchesTopBorder = (row - size < 0);
        if (!touchesTopBorder) {
          total -= sums[row - size][col];
        }

        boolean touchesLeftBorder = (col - size < 0);
        if (!touchesLeftBorder) {
          total -= sums[row][col - size];
        }

        boolean touchesTopOrLeftBorder = (touchesTopBorder || touchesLeftBorder);
        if (!touchesTopOrLeftBorder) {
          total += sums[row - size][col - size];
        }

        maxSubMatrixSum = Math.max(maxSubMatrixSum, total);
      }
    }

    return maxSubMatrixSum;
  }

  public int[][] createSumMatrix(int[][] matrix) {
    int[][] sums = new int[matrix.length][matrix[0].length];
    sums[0][0] = matrix[0][0];

    // Fill the first row.
    for (int idx = 1; idx < matrix[0].length; idx++) {
      sums[0][idx] = sums[0][idx - 1] + matrix[0][idx];
    }

    // Fill the first column.
    for (int idx = 1; idx < matrix.length; idx++) {
      sums[idx][0] = sums[idx - 1][0] + matrix[idx][0];
    }

    // Fill in the rest of the matrix.
    for (int row = 1; row < matrix.length; row++) {
      for (int col = 1; col < matrix[0].length; col++) {
        sums[row][col] =
            sums[row - 1][col] + sums[row][col - 1] - sums[row - 1][col - 1] + matrix[row][col];
      }
    }

    return sums;
  }
}

```
