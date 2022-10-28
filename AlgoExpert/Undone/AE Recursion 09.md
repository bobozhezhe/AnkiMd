# AlgoExpert

## Recursion 09 Solve Sudoku

### Question

You're given a two-dimensional array that represents a 9x9 partially filled Sudoku board. Write a function that returns the solved Sudoku board.

Sudoku is a famous number-placement puzzle in which you need to fill a 9x9 grid with integers in the range of 1-9. Each 9x9 Sudoku board is split into 9 3x3 subgrids, as seen in the illustration below, and starts out partially filled.

```c
- - 3 | - 2 - | 6 - - 
9 - - | 3 - 5 | - - 1 
- - 1 | 8 - 6 | 4 - -
- - - - - - - - - - - 
- - 8 | 1 - 2 | 9 - -
7 - - | - - - | - - 8 
- - 6 | 7 - 8 | 2 - -
- - - - - - - - - - -
- - 2 | 6 - 9 | 5 - - 
8 - - | 2 - 3 | - - 9
- - 5 | - 1 - | 3 - -
```

The objective is to fill the grid such that each row, column, and 3x3 subgrid contains the numbers 1-9 exactly once. In other words, no row may contain the same digit more than once, no column may contain the same digit more than once, and none of the 9 3x3 subgrids may contain the same digit more than once.

Your input for this problem will always be a partially filled 9x9 two-dimensional array that represents a solvable Sudoku puzzle. Every element in the array will be an integer in the range of 0-9, where a 0 represents an empty square that must be filled by your algorithm.

Note that you may modify the input array and that there will always be exactly one solution to each input Sudoku board.

### Sample Input

board =
[
  [7, 8, 0, 4, 0, 0, 1, 2, 0],
  [6, 0, 0, 0, 7, 5, 0, 0, 9],
  [0, 0, 0, 6, 0, 1, 0, 7, 8],
  [0, 0, 7, 0, 4, 0, 2, 6, 0],
  [0, 0, 1, 0, 5, 0, 9, 3, 0],
  [9, 0, 4, 0, 6, 0, 0, 0, 5],
  [0, 7, 0, 3, 0, 0, 0, 1, 2],
  [1, 2, 0, 0, 0, 7, 4, 0, 0],
  [0, 4, 9, 2, 0, 6, 0, 0, 7],
]

### Sample Output

[
  [7, 8, 5, 4, 3, 9, 1, 2, 6],
  [6, 1, 2, 8, 7, 5, 3, 4, 9],
  [4, 9, 3, 6, 2, 1, 5, 7, 8],
  [8, 5, 7, 9, 4, 3, 2, 6, 1],
  [2, 6, 1, 7, 5, 8, 9, 3, 4],
  [9, 3, 4, 1, 6, 2, 7, 8, 5],
  [5, 7, 8, 3, 9, 4, 6, 1, 2],
  [1, 2, 6, 5, 8, 7, 4, 9, 3],
  [3, 4, 9, 2, 1, 6, 8, 5, 7],
]

### Optimal Space & Time Complexity

O(1) time | O(1) space - assuming a 9x9 input board

%

### Key Point

### Solution 1

```java
class Program {

  // O(1) time | O(1) space - assuming a 9x9 input board
  public ArrayList<ArrayList<Integer>> solveSudoku(ArrayList<ArrayList<Integer>> board) {
    solvePartialSudoku(0, 0, board);
    return board;
  }

  public boolean solvePartialSudoku(int row, int col, ArrayList<ArrayList<Integer>> board) {
    int currentRow = row;
    int currentCol = col;

    if (currentCol == board.get(currentRow).size()) {
      currentRow += 1;
      currentCol = 0;
      if (currentRow == board.size()) {
        return true; // board is completed
      }
    }

    if (board.get(currentRow).get(currentCol) == 0) {
      return tryDigitsAtPosition(currentRow, currentCol, board);
    }

    return solvePartialSudoku(currentRow, currentCol + 1, board);
  }

  public boolean tryDigitsAtPosition(int row, int col, ArrayList<ArrayList<Integer>> board) {

    for (int digit = 1; digit < 10; digit++) {
      if (isValidAtPosition(digit, row, col, board)) {
        board.get(row).set(col, digit);
        if (solvePartialSudoku(row, col + 1, board)) {
          return true;
        }
      }
    }

    board.get(row).set(col, 0);
    return false;
  }

  public boolean isValidAtPosition(
      int value, int row, int col, ArrayList<ArrayList<Integer>> board) {
    boolean rowIsValid = !board.get(row).contains(value);
    boolean columnIsValid = true;

    for (int r = 0; r < board.size(); r++) {
      if (board.get(r).get(col) == value) columnIsValid = false;
    }

    if (!rowIsValid || !columnIsValid) {
      return false;
    }

    // Check subgrid constraints
    int subgridRowStart = (row / 3) * 3;
    int subgridColStart = (col / 3) * 3;

    for (int rowIdx = 0; rowIdx < 3; rowIdx++) {
      for (int colIdx = 0; colIdx < 3; colIdx++) {
        int rowToCheck = subgridRowStart + rowIdx;
        int colToCheck = subgridColStart + colIdx;
        int existingValue = board.get(rowToCheck).get(colToCheck);

        if (existingValue == value) {
          return false;
        }
      }
    }

    return true;
  }
}

```
