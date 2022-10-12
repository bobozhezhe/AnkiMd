# AlgoExpert

## Searching 01 Binary Search

### Question

Write a function that takes in a sorted array of integers as well as a target integer. The function should use the Binary Search algorithm to determine if the target integer is contained in the array and should return its index if it is, otherwise -1.

If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

array = [0, 1, 21, 33, 45, 45, 61, 71, 72, 73]
target = 33

### Sample Output

3

### Optimal Space & Time Complexity

O(log(n)) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(log(n)) time | O(log(n)) space
  public static int binarySearch(int[] array, int target) {
    return binarySearch(array, target, 0, array.length - 1);
  }

  public static int binarySearch(int[] array, int target, int left, int right) {
    if (left > right) {
      return -1;
    }
    int middle = (left + right) / 2;
    int potentialMatch = array[middle];
    if (target == potentialMatch) {
      return middle;
    } else if (target < potentialMatch) {
      return binarySearch(array, target, left, middle - 1);
    } else {
      return binarySearch(array, target, middle + 1, right);
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(log(n)) time | O(1) space
  public static int binarySearch(int[] array, int target) {
    return binarySearch(array, target, 0, array.length - 1);
  }

  public static int binarySearch(int[] array, int target, int left, int right) {
    while (left <= right) {
      int middle = (left + right) / 2;
      int potentialMatch = array[middle];
      if (target == potentialMatch) {
        return middle;
      } else if (target < potentialMatch) {
        right = middle - 1;
      } else {
        left = middle + 1;
      }
    }
    return -1;
  }
}

```
