# AlgoExpert

## Arrays 09 Monotonic Array

### Question

Write a function that takes in an array of integers and returns a boolean representing whether the array is monotonic.

An array is said to be monotonic if its elements, from left to right, are entirely non-increasing or entirely non-decreasing.

Non-increasing elements aren't necessarily exclusively decreasing; they simply don't increase. Similarly, non-decreasing elements aren't necessarily exclusively increasing; they simply don't decrease.

Note that empty arrays and arrays of one element are monotonic.

### Sample Input

array = [-1, -5, -10, -1100, -1100, -1101, -1102, -9001]

### Sample Output

true

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the array
  public static boolean isMonotonic(int[] array) {
    if (array.length <= 2) return true;

    var direction = array[1] - array[0];
    for (int i = 2; i < array.length; i++) {
      if (direction == 0) {
        direction = array[i] - array[i - 1];
        continue;
      }

      if (breaksDirection(direction, array[i - 1], array[i])) {
        return false;
      }
    }
    return true;
  }

  public static boolean breaksDirection(int direction, int previous, int current) {
    var difference = current - previous;
    if (direction > 0) return difference < 0;
    return difference > 0;
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the array
  public static boolean isMonotonic(int[] array) {
    var isNonDecreasing = true;
    var isNonIncreasing = true;
    for (int i = 1; i < array.length; i++) {
      if (array[i] < array[i - 1]) {
        isNonDecreasing = false;
      }
      if (array[i] > array[i - 1]) {
        isNonIncreasing = false;
      }
    }

    return isNonDecreasing || isNonIncreasing;
  }
}

```
