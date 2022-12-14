# AlgoExpert

## Searching 05 Search For Range

### Question

Write a function that takes in a sorted array of integers as well as a target integer. The function should use a variation of the Binary Search algorithm to find a range of indices in between which the target number is contained in the array and should return this range in the form of an array.

The first number in the output array should represent the first index at which the target number is located, while the second number should represent the last index at which the target number is located. The function should return [-1, -1] if the integer isn't contained in the array.

If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of the Binary Search question's video explanation before starting to code.

### Sample Input

array = [0, 1, 21, 33, 45, 45, 45, 45, 45, 45, 61, 71, 73]
target = 45

### Sample Output

[4, 9]

### Optimal Space & Time Complexity

O(log(n)) time | O(1) space - where n is the length of the input array
%

### Key Point

### Solution 1

```java
class Program {
  // O(log(n)) time | O(log(n)) space
  public static int[] searchForRange(int[] array, int target) {
    int[] finalRange = {-1, -1};
    alteredBinarySearch(array, target, 0, array.length - 1, finalRange, true);
    alteredBinarySearch(array, target, 0, array.length - 1, finalRange, false);
    return finalRange;
  }

  public static void alteredBinarySearch(
      int[] array, int target, int left, int right, int[] finalRange, boolean goLeft) {
    if (left > right) {
      return;
    }
    int mid = (left + right) / 2;
    if (array[mid] < target) {
      alteredBinarySearch(array, target, mid + 1, right, finalRange, goLeft);
    } else if (array[mid] > target) {
      alteredBinarySearch(array, target, left, mid - 1, finalRange, goLeft);
    } else {
      if (goLeft) {
        if (mid == 0 || array[mid - 1] != target) {
          finalRange[0] = mid;
        } else {
          alteredBinarySearch(array, target, left, mid - 1, finalRange, goLeft);
        }
      } else {
        if (mid == array.length - 1 || array[mid + 1] != target) {
          finalRange[1] = mid;
        } else {
          alteredBinarySearch(array, target, mid + 1, right, finalRange, goLeft);
        }
      }
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(log(n)) time | O(1) space
  public static int[] searchForRange(int[] array, int target) {
    int[] finalRange = {-1, -1};
    alteredBinarySearch(array, target, 0, array.length - 1, finalRange, true);
    alteredBinarySearch(array, target, 0, array.length - 1, finalRange, false);
    return finalRange;
  }

  public static void alteredBinarySearch(
      int[] array, int target, int left, int right, int[] finalRange, boolean goLeft) {
    while (left <= right) {
      int mid = (left + right) / 2;
      if (array[mid] < target) {
        left = mid + 1;
      } else if (array[mid] > target) {
        right = mid - 1;
      } else {
        if (goLeft) {
          if (mid == 0 || array[mid - 1] != target) {
            finalRange[0] = mid;
            return;
          } else {
            right = mid - 1;
          }
        } else {
          if (mid == array.length - 1 || array[mid + 1] != target) {
            finalRange[1] = mid;
            return;
          } else {
            left = mid + 1;
          }
        }
      }
    }
  }
}

```
