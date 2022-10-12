# AlgoExpert

## Searching 04 Shifted Binary Search

### Question

Write a function that takes in a sorted array of distinct integers as well as a target integer. The caveat is that the integers in the array have been shifted by some amount; in other words, they've been moved to the left or to the right by one or more positions. For example, [1, 2, 3, 4] might have turned into [3, 4, 1, 2].

The function should use a variation of the Binary Search algorithm to determine if the target integer is contained in the array and should return its index if it is, otherwise -1.

If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of the Binary Search question's video explanation before starting to code.

### Sample Input

array = [45, 61, 71, 72, 73, 0, 1, 21, 33, 37]
target = 33

### Sample Output

8

### Optimal Space & Time Complexity

O(log(n)) time | O(1) space - where n is the length of the input array
%

### Key Point

### Solution 1

```java
class Program {
  // O(log(n)) time | O(log(n)) space
  public static int shiftedBinarySearch(int[] array, int target) {
    return shiftedBinarySearch(array, target, 0, array.length - 1);
  }

  public static int shiftedBinarySearch(int[] array, int target, int left, int right) {
    if (left > right) {
      return -1;
    }
    int middle = (left + right) / 2;
    int potentialMatch = array[middle];
    int leftNum = array[left];
    int rightNum = array[right];
    if (target == potentialMatch) {
      return middle;
    } else if (leftNum <= potentialMatch) {
      if (target < potentialMatch && target >= leftNum) {
        return shiftedBinarySearch(array, target, left, middle - 1);
      } else {
        return shiftedBinarySearch(array, target, middle + 1, right);
      }
    } else {
      if (target > potentialMatch && target <= rightNum) {
        return shiftedBinarySearch(array, target, middle + 1, right);
      } else {
        return shiftedBinarySearch(array, target, left, middle - 1);
      }
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(log(n)) time | O(1) space
  public static int shiftedBinarySearch(int[] array, int target) {
    return shiftedBinarySearch(array, target, 0, array.length - 1);
  }

  public static int shiftedBinarySearch(int[] array, int target, int left, int right) {
    while (left <= right) {
      int middle = (left + right) / 2;
      int potentialMatch = array[middle];
      int leftNum = array[left];
      int rightNum = array[right];
      if (target == potentialMatch) {
        return middle;
      } else if (leftNum <= potentialMatch) {
        if (target < potentialMatch && target >= leftNum) {
          right = middle - 1;
        } else {
          left = middle + 1;
        }
      } else {
        if (target > potentialMatch && target <= rightNum) {
          left = middle + 1;
        } else {
          right = middle - 1;
        }
      }
    }
    return -1;
  }
}

```
