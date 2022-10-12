# AlgoExpert

## Searching 06 Quickselect

### Question

Write a function that takes in an array of distinct integers as well as an integer k and that returns the kth smallest integer in that array.

The function should do this in linear time, on average.

### Sample Input

array = [8, 5, 2, 9, 7, 6, 3]
k = 3

### Sample Output

5

### Optimal Space & Time Complexity

Best: O(n) time | O(1) space - where n is the length of the input array Average: O(n) time | O(1) space - where n is the length of the input array Worst: O(n^2) time | O(1) space - where n is the length of the input array
%

### Key Point

### Solution 1

```java
class Program {
  // Best: O(n) time | O(1) space
  // Average: O(n) time | O(1) space
  // Worst: O(n^2) time | O(1) space
  public static int quickselect(int[] array, int k) {
    int position = k - 1;
    return quickselect(array, 0, array.length - 1, position);
  }

  public static int quickselect(int[] array, int startIdx, int endIdx, int position) {
    while (true) {
      if (startIdx > endIdx) {
        throw new RuntimeException("Your Algorithm should never arrive here!");
      }
      int pivotIdx = startIdx;
      int leftIdx = startIdx + 1;
      int rightIdx = endIdx;
      while (leftIdx <= rightIdx) {
        if (array[leftIdx] > array[pivotIdx] && array[rightIdx] < array[pivotIdx]) {
          swap(leftIdx, rightIdx, array);
        }
        if (array[leftIdx] <= array[pivotIdx]) {
          leftIdx++;
        }
        if (array[rightIdx] >= array[pivotIdx]) {
          rightIdx--;
        }
      }
      swap(pivotIdx, rightIdx, array);
      if (rightIdx == position) {
        return array[rightIdx];
      } else if (rightIdx < position) {
        startIdx = rightIdx + 1;
      } else {
        endIdx = rightIdx - 1;
      }
    }
  }

  public static void swap(int i, int j, int[] array) {
    int temp = array[j];
    array[j] = array[i];
    array[i] = temp;
  }
}

```
