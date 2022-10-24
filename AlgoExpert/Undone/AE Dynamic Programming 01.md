# AlgoExpert

## Dynamic Programming 01 Max Subset Sum No Adjacent

### Question

Write a function that takes in an array of positive integers and returns the maximum sum of non-adjacent elements in the array.

If the input array is empty, the function should return 0.

### Sample Input

array = [75, 105, 120, 75, 90, 135]

### Sample Output

330 // 75 + 120 + 135

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space
  public static int maxSubsetSumNoAdjacent(int[] array) {
    if (array.length == 0) {
      return 0;
    } else if (array.length == 1) {
      return array[0];
    }
    int[] maxSums = array.clone();
    maxSums[1] = Math.max(array[0], array[1]);
    for (int i = 2; i < array.length; i++) {
      maxSums[i] = Math.max(maxSums[i - 1], maxSums[i - 2] + array[i]);
    }
    return maxSums[array.length - 1];
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(1) space
  public static int maxSubsetSumNoAdjacent(int[] array) {
    if (array.length == 0) {
      return 0;
    } else if (array.length == 1) {
      return array[0];
    }
    int second = array[0];
    int first = Math.max(array[0], array[1]);
    for (int i = 2; i < array.length; i++) {
      int current = Math.max(first, second + array[i]);
      second = first;
      first = current;
    }
    return first;
  }
}

```
