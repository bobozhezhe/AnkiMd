# AlgoExpert

## Dynamic Programming 19 Square of Zeroes

### Question

Write a function that takes in a square-shaped n x n two-dimensional array of only 1s and 0s and returns a boolean representing whether the input matrix contains a square whose borders are made up of only 0s.

Note that a 1 x 1 square doesn't count as a valid square for the purpose of this question. In other words, a singular 0 in the input matrix doesn't constitute a square whose borders are made up of only 0s; a square of zeroes has to be at least 2 x 2.

### Sample Input

matrix = [
  [1, 1, 1, 0, 1, 0],
  [0, 0, 0, 0, 0, 1],
  [0, 1, 1, 1, 0, 1],
  [0, 0, 0, 1, 0, 1],
  [0, 1, 1, 1, 0, 1],
  [0, 0, 0, 0, 0, 1],
]

### Sample Output

true
[
  [ ,  ,  ,  ,  ,  ],
  [0, 0, 0, 0, 0,  ],
  [0,  ,  ,  , 0,  ],
  [0,  ,  ,  , 0,  ],
  [0,  ,  ,  , 0,  ],
  [0, 0, 0, 0, 0,  ],
]

### Optimal Space & Time Complexity

O(n^3) time | O(n^2) space - where n is the height and width of the matrix

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^4) time | O(n^3) space - where n is the height and width of the matrix
  public static boolean squareOfZeroes(List<List<Integer>> matrix) {
    int lastIdx = matrix.size() - 1;
    Map<String, Boolean> cache = new HashMap<String, Boolean>();
    return hasSquareOfZeroes(matrix, 0, 0, lastIdx, lastIdx, cache);
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean hasSquareOfZeroes(
      List<List<Integer>> matrix, int r1, int c1, int r2, int c2, Map<String, Boolean> cache) {
    if (r1 >= r2 || c1 >= c2) return false;

    String key =
        String.valueOf(r1)
            + '-'
            + String.valueOf(c1)
            + '-'
            + String.valueOf(r2)
            + '-'
            + String.valueOf(c2);
    if (cache.containsKey(key)) return cache.get(key);

    cache.put(
        key,
        isSquareOfZeroes(matrix, r1, c1, r2, c2)
            || hasSquareOfZeroes(matrix, r1 + 1, c1 + 1, r2 - 1, c2 - 1, cache)
            || hasSquareOfZeroes(matrix, r1, c1 + 1, r2 - 1, c2, cache)
            || hasSquareOfZeroes(matrix, r1 + 1, c1, r2, c2 - 1, cache)
            || hasSquareOfZeroes(matrix, r1 + 1, c1 + 1, r2, c2, cache)
            || hasSquareOfZeroes(matrix, r1, c1, r2 - 1, c2 - 1, cache));

    return cache.get(key);
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean isSquareOfZeroes(
      List<List<Integer>> matrix, int r1, int c1, int r2, int c2) {
    for (int row = r1; row < r2 + 1; row++) {
      if (matrix.get(row).get(c1) != 0 || matrix.get(row).get(c2) != 0) return false;
    }
    for (int col = c1; col < c2 + 1; col++) {
      if (matrix.get(r1).get(col) != 0 || matrix.get(r2).get(col) != 0) return false;
    }
    return true;
  }
}

```

### Solution 2

```java
class Program {
  // O(n^4) time | O(1) space - where n is the height and width of the matrix
  public static boolean squareOfZeroes(List<List<Integer>> matrix) {
    int n = matrix.size();
    for (int topRow = 0; topRow < n; topRow++) {
      for (int leftCol = 0; leftCol < n; leftCol++) {
        int squareLength = 2;
        while (squareLength <= n - leftCol && squareLength <= n - topRow) {
          int bottomRow = topRow + squareLength - 1;
          int rightCol = leftCol + squareLength - 1;
          if (isSquareOfZeroes(matrix, topRow, leftCol, bottomRow, rightCol)) return true;
          squareLength++;
        }
      }
    }
    return false;
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean isSquareOfZeroes(
      List<List<Integer>> matrix, int r1, int c1, int r2, int c2) {
    for (int row = r1; row < r2 + 1; row++) {
      if (matrix.get(row).get(c1) != 0 || matrix.get(row).get(c2) != 0) return false;
    }
    for (int col = c1; col < c2 + 1; col++) {
      if (matrix.get(r1).get(col) != 0 || matrix.get(r2).get(col) != 0) return false;
    }
    return true;
  }
}

```

### Solution 3

```java
class Program {
  // O(n^3) time | O(n^3) space - where n is the height and width of the matrix
  public static boolean squareOfZeroes(List<List<Integer>> matrix) {
    List<List<InfoMatrixItem>> infoMatrix = preComputedNumOfZeroes(matrix);
    int lastIdx = matrix.size() - 1;
    Map<String, Boolean> cache = new HashMap<String, Boolean>();
    return hasSquareOfZeroes(infoMatrix, 0, 0, lastIdx, lastIdx, cache);
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean hasSquareOfZeroes(
      List<List<InfoMatrixItem>> matrix,
      int r1,
      int c1,
      int r2,
      int c2,
      Map<String, Boolean> cache) {
    if (r1 >= r2 || c1 >= c2) return false;

    String key =
        String.valueOf(r1)
            + '-'
            + String.valueOf(c1)
            + '-'
            + String.valueOf(r2)
            + '-'
            + String.valueOf(c2);
    if (cache.containsKey(key)) return cache.get(key);

    cache.put(
        key,
        isSquareOfZeroes(matrix, r1, c1, r2, c2)
            || hasSquareOfZeroes(matrix, r1 + 1, c1 + 1, r2 - 1, c2 - 1, cache)
            || hasSquareOfZeroes(matrix, r1, c1 + 1, r2 - 1, c2, cache)
            || hasSquareOfZeroes(matrix, r1 + 1, c1, r2, c2 - 1, cache)
            || hasSquareOfZeroes(matrix, r1 + 1, c1 + 1, r2, c2, cache)
            || hasSquareOfZeroes(matrix, r1, c1, r2 - 1, c2 - 1, cache));

    return cache.get(key);
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean isSquareOfZeroes(
      List<List<InfoMatrixItem>> infoMatrix, int r1, int c1, int r2, int c2) {
    int squareLength = c2 - c1 + 1;
    boolean hasTopBorder = infoMatrix.get(r1).get(c1).numZeroesRight >= squareLength;
    boolean hasLeftBorder = infoMatrix.get(r1).get(c1).numZeroesBelow >= squareLength;
    boolean hasBottomBorder = infoMatrix.get(r2).get(c1).numZeroesRight >= squareLength;
    boolean hasRightBorder = infoMatrix.get(r1).get(c2).numZeroesBelow >= squareLength;
    return hasTopBorder && hasLeftBorder && hasBottomBorder && hasRightBorder;
  }

  public static List<List<InfoMatrixItem>> preComputedNumOfZeroes(List<List<Integer>> matrix) {
    List<List<InfoMatrixItem>> infoMatrix = new ArrayList<List<InfoMatrixItem>>();
    for (int i = 0; i < matrix.size(); i++) {
      List<InfoMatrixItem> inner = new ArrayList<InfoMatrixItem>();
      for (int j = 0; j < matrix.get(i).size(); j++) {
        int numZeroes = matrix.get(i).get(j) == 0 ? 1 : 0;
        inner.add(new InfoMatrixItem(numZeroes, numZeroes));
      }
      infoMatrix.add(inner);
    }

    int lastIdx = matrix.size() - 1;
    for (int row = lastIdx; row >= 0; row--) {
      for (int col = lastIdx; col >= 0; col--) {
        if (matrix.get(row).get(col) == 1) continue;
        if (row < lastIdx) {
          infoMatrix.get(row).get(col).numZeroesBelow +=
              infoMatrix.get(row + 1).get(col).numZeroesBelow;
        }
        if (col < lastIdx) {
          infoMatrix.get(row).get(col).numZeroesRight +=
              infoMatrix.get(row).get(col + 1).numZeroesRight;
        }
      }
    }

    return infoMatrix;
  }

  static class InfoMatrixItem {
    public int numZeroesBelow;
    public int numZeroesRight;

    public InfoMatrixItem(int numZeroesBelow, int numZeroesRight) {
      this.numZeroesBelow = numZeroesBelow;
      this.numZeroesRight = numZeroesRight;
    }
  }
}

```

### Solution 4

```java
class Program {
  // O(n^3) time | O(n^2) space - where n is the height and width of the matrix
  public static boolean squareOfZeroes(List<List<Integer>> matrix) {
    List<List<InfoMatrixItem>> infoMatrix = preComputedNumOfZeroes(matrix);
    int n = matrix.size();
    for (int topRow = 0; topRow < n; topRow++) {
      for (int leftCol = 0; leftCol < n; leftCol++) {
        int squareLength = 2;
        while (squareLength <= n - leftCol && squareLength <= n - topRow) {
          int bottomRow = topRow + squareLength - 1;
          int rightCol = leftCol + squareLength - 1;
          if (isSquareOfZeroes(infoMatrix, topRow, leftCol, bottomRow, rightCol)) return true;
          squareLength++;
        }
      }
    }
    return false;
  }

  // r1 is the top row, c1 is the left column
  // r2 is the bottom row, c2 is the right column
  public static boolean isSquareOfZeroes(
      List<List<InfoMatrixItem>> infoMatrix, int r1, int c1, int r2, int c2) {
    int squareLength = c2 - c1 + 1;
    boolean hasTopBorder = infoMatrix.get(r1).get(c1).numZeroesRight >= squareLength;
    boolean hasLeftBorder = infoMatrix.get(r1).get(c1).numZeroesBelow >= squareLength;
    boolean hasBottomBorder = infoMatrix.get(r2).get(c1).numZeroesRight >= squareLength;
    boolean hasRightBorder = infoMatrix.get(r1).get(c2).numZeroesBelow >= squareLength;
    return hasTopBorder && hasLeftBorder && hasBottomBorder && hasRightBorder;
  }

  public static List<List<InfoMatrixItem>> preComputedNumOfZeroes(List<List<Integer>> matrix) {
    List<List<InfoMatrixItem>> infoMatrix = new ArrayList<List<InfoMatrixItem>>();
    for (int i = 0; i < matrix.size(); i++) {
      List<InfoMatrixItem> inner = new ArrayList<InfoMatrixItem>();
      for (int j = 0; j < matrix.get(i).size(); j++) {
        int numZeroes = matrix.get(i).get(j) == 0 ? 1 : 0;
        inner.add(new InfoMatrixItem(numZeroes, numZeroes));
      }
      infoMatrix.add(inner);
    }

    int lastIdx = matrix.size() - 1;
    for (int row = lastIdx; row >= 0; row--) {
      for (int col = lastIdx; col >= 0; col--) {
        if (matrix.get(row).get(col) == 1) continue;
        if (row < lastIdx) {
          infoMatrix.get(row).get(col).numZeroesBelow +=
              infoMatrix.get(row + 1).get(col).numZeroesBelow;
        }
        if (col < lastIdx) {
          infoMatrix.get(row).get(col).numZeroesRight +=
              infoMatrix.get(row).get(col + 1).numZeroesRight;
        }
      }
    }

    return infoMatrix;
  }

  static class InfoMatrixItem {
    public int numZeroesBelow;
    public int numZeroesRight;

    public InfoMatrixItem(int numZeroesBelow, int numZeroesRight) {
      this.numZeroesBelow = numZeroesBelow;
      this.numZeroesRight = numZeroesRight;
    }
  }
}

```
