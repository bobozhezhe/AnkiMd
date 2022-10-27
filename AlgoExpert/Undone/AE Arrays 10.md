# AlgoExpert

## Arrays 10 Spiral Traverse

### Question

Write a function that takes in an n x m two-dimensional array (that can be square-shaped when n == m) and returns a one-dimensional array of all the array's elements in spiral order.

Spiral order starts at the top left corner of the two-dimensional array, goes to the right, and proceeds in a spiral pattern all the way until every element has been visited.

### Sample Input

array = [
  [1,   2,  3, 4],
  [12, 13, 14, 5],
  [11, 16, 15, 6],
  [10,  9,  8, 7],
]

### Sample Output

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the total number of elements in the array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space - where n is the total number of elements in the array
  public static List<Integer> spiralTraverse(int[][] array) {
    if (array.length == 0) return new ArrayList<Integer>();

    var result = new ArrayList<Integer>();
    var startRow = 0;
    var endRow = array.length - 1;
    var startCol = 0;
    var endCol = array[0].length - 1;

    while (startRow <= endRow && startCol <= endCol) {
      for (int col = startCol; col <= endCol; col++) {
        result.add(array[startRow][col]);
      }

      for (int row = startRow + 1; row <= endRow; row++) {
        result.add(array[row][endCol]);
      }

      for (int col = endCol - 1; col >= startCol; col--) {
        // Handle the edge case when there's a single row
        // in the middle of the matrix. In this case, we don't
        // want to double-count the values in this row, which
        // we've already counted in the first for loop above.
        // See Test Case 8 for an example of this edge case.
        if (startRow == endRow) break;
        result.add(array[endRow][col]);
      }

      for (int row = endRow - 1; row > startRow; row--) {
        // Handle the edge case when there's a single column
        // in the middle of the matrix. In this case, we don't
        // want to double-count the values in this column, which
        // we've already counted in the second for loop above.
        // See Test Case 9 for an example of this edge case.
        if (startCol == endCol) break;
        result.add(array[row][startCol]);
      }

      startRow++;
      endRow--;
      startCol++;
      endCol--;
    }
    return result;
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space - where n is the total number of elements in the array
  public static List<Integer> spiralTraverse(int[][] array) {
    if (array.length == 0) return new ArrayList<Integer>();

    var result = new ArrayList<Integer>();
    spiralFill(array, 0, array.length - 1, 0, array[0].length - 1, result);
    return result;
  }

  public static void spiralFill(
      int[][] array,
      int startRow,
      int endRow,
      int startCol,
      int endCol,
      ArrayList<Integer> result) {
    if (startRow > endRow || startCol > endCol) {
      return;
    }

    for (int col = startCol; col <= endCol; col++) {
      result.add(array[startRow][col]);
    }

    for (int row = startRow + 1; row <= endRow; row++) {
      result.add(array[row][endCol]);
    }

    for (int col = endCol - 1; col >= startCol; col--) {
      // Handle the edge case when there's a single row
      // in the middle of the matrix. In this case, we don't
      // want to double-count the values in this row, which
      // we've already counted in the first for loop above.
      // See Test Case 8 for an example of this edge case.
      if (startRow == endRow) break;
      result.add(array[endRow][col]);
    }

    for (int row = endRow - 1; row >= startRow + 1; row--) {
      // Handle the edge case when there's a single column
      // in the middle of the matrix. In this case, we don't
      // want to double-count the values in this column, which
      // we've already counted in the second for loop above.
      // See Test Case 9 for an example of this edge case.
      if (startCol == endCol) break;
      result.add(array[row][startCol]);
    }
    spiralFill(array, startRow + 1, endRow - 1, startCol + 1, endCol - 1, result);
  }
}

```
