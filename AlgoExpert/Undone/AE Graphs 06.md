# AlgoExpert

## Graphs 06 Remove Islands

### Question

You're given a two-dimensional array (a matrix) of potentially unequal height and width containing only 0s and 1s. The matrix represents a two-toned image, where each 1 represents black and each 0 represents white. An island is defined as any number of 1s that are horizontally or vertically adjacent (but not diagonally adjacent) and that don't touch the border of the image. In other words, a group of horizontally or vertically adjacent 1s isn't an island if any of those 1s are in the first row, last row, first column, or last column of the input matrix.

Note that an island can twist. In other words, it doesn't have to be a straight vertical line or a straight horizontal line; it can be L-shaped, for example.

You can think of islands as patches of black that don't touch the border of the two-toned image.

Write a function that returns a modified version of the input matrix, where all of the islands are removed. You remove an island by replacing it with 0s.

Naturally, you're allowed to mutate the input matrix.

### Sample Input

matrix =
[
  [1, 0, 0, 0, 0, 0],
  [0, 1, 0, 1, 1, 1],
  [0, 0, 1, 0, 1, 0],
  [1, 1, 0, 0, 1, 0],
  [1, 0, 1, 1, 0, 0],
  [1, 0, 0, 0, 0, 1],
]

### Sample Output

[
  [1, 0, 0, 0, 0, 0],
  [0, 0, 0, 1, 1, 1],
  [0, 0, 0, 0, 1, 0],
  [1, 1, 0, 0, 1, 0],
  [1, 0, 0, 0, 0, 0],
  [1, 0, 0, 0, 0, 1],
]
// The islands that were removed can be clearly seen here:
// [
//   [ ,  ,  ,  ,  , ],
//   [ , 1,  ,  ,  , ],
//   [ ,  , 1,  ,  , ],
//   [ ,  ,  ,  ,  , ],
//   [ ,  , 1, 1,  , ],
//   [ ,  ,  ,  ,  , ],
// ]

### Optimal Space & Time Complexity

O(wh) time | O(wh) space - where w and h are the width and height of the input matrix

%

### Key Point

### Solution 1

```java
class Program {

  // O(wh) time | O(wh) space - where w and h
  // are the width and height of the input matrix
  int[][] removeIslands(int[][] matrix) {
    boolean[][] onesConnectedToBorder = new boolean[matrix.length][matrix[0].length];
    for (int i = 0; i < matrix.length; i++) {
      onesConnectedToBorder[i][matrix[0].length - 1] = false;
    }

    // Find all the 1s that are not islands
    for (int row = 0; row < matrix.length; row++) {
      for (int col = 0; col < matrix[row].length; col++) {
        boolean rowIsBorder = row == 0 || row == matrix.length - 1;
        boolean colIsBorder = col == 0 || col == matrix[row].length - 1;
        boolean isBorder = rowIsBorder || colIsBorder;

        if (!isBorder) {
          continue;
        }

        if (matrix[row][col] != 1) {
          continue;
        }

        findOnesConnectedToBorder(matrix, row, col, onesConnectedToBorder);
      }
    }

    for (int row = 1; row < matrix.length - 1; row++) {
      for (int col = 1; col < matrix[row].length - 1; col++) {
        if (onesConnectedToBorder[row][col]) {
          continue;
        }
        matrix[row][col] = 0;
      }
    }

    return matrix;
  }

  public void findOnesConnectedToBorder(
      int[][] matrix, int startRow, int startCol, boolean[][] onesConnectedToBorder) {
    Stack<int[]> stack = new Stack<int[]>();
    stack.push(new int[] {startRow, startCol});

    while (stack.size() > 0) {
      int[] currentPosition = stack.pop();
      int currentRow = currentPosition[0];
      int currentCol = currentPosition[1];

      boolean alreadyVisited = onesConnectedToBorder[currentRow][currentCol];
      if (alreadyVisited) {
        continue;
      }

      onesConnectedToBorder[currentRow][currentCol] = true;

      int[][] neighbors = getNeighbors(matrix, currentRow, currentCol);
      for (int[] neighbor : neighbors) {
        int row = neighbor[0];
        int col = neighbor[1];

        if (matrix[row][col] != 1) {
          continue;
        }
        stack.push(neighbor);
      }
    }
  }

  public int[][] getNeighbors(int[][] matrix, int row, int col) {
    int numRows = matrix.length;
    int numCols = matrix[row].length;
    ArrayList<int[]> temp = new ArrayList<int[]>();

    if (row - 1 >= 0) {
      temp.add(new int[] {row - 1, col}); // UP
    }
    if (row + 1 < numRows) {
      temp.add(new int[] {row + 1, col}); // DOWN
    }
    if (col - 1 >= 0) {
      temp.add(new int[] {row, col - 1}); // LEFT
    }
    if (col + 1 < numCols) {
      temp.add(new int[] {row, col + 1}); // RIGHT
    }

    int[][] neighbors = new int[temp.size()][2];
    for (int i = 0; i < temp.size(); i++) {
      neighbors[i] = temp.get(i);
    }
    return neighbors;
  }
}

```

### Solution 2

```java
class Program {

  // O(wh) time | O(wh) space - where w and h
  // are the width and height of the input matrix
  int[][] removeIslands(int[][] matrix) {

    for (int row = 0; row < matrix.length; row++) {
      for (int col = 0; col < matrix[row].length; col++) {
        boolean rowIsBorder = row == 0 || row == matrix.length - 1;
        boolean colIsBorder = col == 0 || col == matrix[row].length - 1;
        boolean isBorder = rowIsBorder || colIsBorder;

        if (!isBorder) {
          continue;
        }

        if (matrix[row][col] != 1) {
          continue;
        }

        changeOnesConnectedToBorderToTwos(matrix, row, col);
      }
    }

    for (int row = 0; row < matrix.length; row++) {
      for (int col = 0; col < matrix[row].length; col++) {
        int color = matrix[row][col];
        if (color == 1) {
          matrix[row][col] = 0;
        } else if (color == 2) {
          matrix[row][col] = 1;
        }
      }
    }

    return matrix;
  }

  public void changeOnesConnectedToBorderToTwos(int[][] matrix, int startRow, int startCol) {
    Stack<int[]> stack = new Stack<int[]>();
    stack.push(new int[] {startRow, startCol});

    while (stack.size() > 0) {
      int[] currentPosition = stack.pop();
      int currentRow = currentPosition[0];
      int currentCol = currentPosition[1];

      matrix[currentRow][currentCol] = 2;

      int[][] neighbors = getNeighbors(matrix, currentRow, currentCol);
      for (int[] neighbor : neighbors) {
        int row = neighbor[0];
        int col = neighbor[1];

        if (matrix[row][col] != 1) {
          continue;
        }
        stack.push(neighbor);
      }
    }
  }

  public int[][] getNeighbors(int[][] matrix, int row, int col) {
    int numRows = matrix.length;
    int numCols = matrix[row].length;
    ArrayList<int[]> temp = new ArrayList<int[]>();

    if (row - 1 >= 0) {
      temp.add(new int[] {row - 1, col}); // UP
    }
    if (row + 1 < numRows) {
      temp.add(new int[] {row + 1, col}); // DOWN
    }
    if (col - 1 >= 0) {
      temp.add(new int[] {row, col - 1}); // LEFT
    }
    if (col + 1 < numCols) {
      temp.add(new int[] {row, col + 1}); // RIGHT
    }

    int[][] neighbors = new int[temp.size()][2];
    for (int i = 0; i < temp.size(); i++) {
      neighbors[i] = temp.get(i);
    }
    return neighbors;
  }
}

```
