# AlgoExpert

## Arrays 16 Subarray Sort

### Question

Write a function that takes in an array of at least two integers and that returns an array of the starting and ending indices of the smallest subarray in the input array that needs to be sorted in place in order for the entire input array to be sorted (in ascending order).

If the input array is already sorted, the function should return [-1, -1].

### Sample Input

array = [1, 2, 4, 7, 10, 11, 7, 12, 6, 7, 16, 18, 19]

### Sample Output

[3, 9]

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space
  public static int[] subarraySort(int[] array) {
    int minOutOfOrder = Integer.MAX_VALUE;
    int maxOutOfOrder = Integer.MIN_VALUE;
    for (int i = 0; i < array.length; i++) {
      int num = array[i];
      if (isOutOfOrder(i, num, array)) {
        minOutOfOrder = Math.min(minOutOfOrder, num);
        maxOutOfOrder = Math.max(maxOutOfOrder, num);
      }
    }
    if (minOutOfOrder == Integer.MAX_VALUE) {
      return new int[] {-1, -1};
    }
    int subarrayLeftIdx = 0;
    while (minOutOfOrder >= array[subarrayLeftIdx]) {
      subarrayLeftIdx++;
    }
    int subarrayRightIdx = array.length - 1;
    while (maxOutOfOrder <= array[subarrayRightIdx]) {
      subarrayRightIdx--;
    }
    return new int[] {subarrayLeftIdx, subarrayRightIdx};
  }

  public static boolean isOutOfOrder(int i, int num, int[] array) {
    if (i == 0) {
      return num > array[i + 1];
    }
    if (i == array.length - 1) {
      return num < array[i - 1];
    }
    return num > array[i + 1] || num < array[i - 1];
  }
}

```
