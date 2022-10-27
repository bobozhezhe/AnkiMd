# AlgoExpert

## Arrays 03 Sorted Squared Array

### Question

Write a function that takes in a non-empty array of integers that are sorted in ascending order and returns a new array of the same length with the squares of the original integers also sorted in ascending order.

### Sample Input

array = [1, 2, 3, 5, 6, 8, 9]

### Sample Output

[1, 4, 9, 25, 36, 64, 81]

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {

  // O(nlogn) time | O(n) space - where n is the length of the input array
  public int[] sortedSquaredArray(int[] array) {
    int[] sortedSquares = new int[array.length];
    for (int idx = 0; idx < array.length; idx++) {
      int value = array[idx];
      sortedSquares[idx] = value * value;
    }
    Arrays.sort(sortedSquares);
    return sortedSquares;
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(n) space - where n is the length of the input array
  public int[] sortedSquaredArray(int[] array) {
    int[] sortedSquares = new int[array.length];
    int smallerValueIdx = 0;
    int largerValueIdx = array.length - 1;
    for (int idx = array.length - 1; idx >= 0; idx--) {
      int smallerValue = array[smallerValueIdx];
      int largerValue = array[largerValueIdx];
      if (Math.abs(smallerValue) > Math.abs(largerValue)) {
        sortedSquares[idx] = smallerValue * smallerValue;
        smallerValueIdx++;
      } else {
        sortedSquares[idx] = largerValue * largerValue;
        largerValueIdx--;
      }
    }
    return sortedSquares;
  }
}

```
