# AlgoExpert

## Sorting 06 Heap Sort

### Question

Write a function that takes in an array of integers and returns a sorted version of that array. Use the Heap Sort algorithm to sort the array.

If you're unfamiliar with Heap Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

array = [8, 5, 2, 9, 5, 6, 3]

### Sample Output

[2, 3, 5, 5, 6, 8, 9]

### Optimal Space & Time Complexity

Best: O(nlog(n)) time | O(1) space - where n is the length of the input array Average: O(nlog(n)) time | O(1) space - where n is the length of the input array Worst: O(nlog(n)) time | O(1) space - where n is the length of the input array
%

### Key Point

### Solution 1

```java
class Program {
  // Best: O(nlog(n)) time | O(1) space
  // Average: O(nlog(n)) time | O(1) space
  // Worst: O(nlog(n)) time | O(1) space
  public static int[] heapSort(int[] array) {
    buildMaxHeap(array);
    for (int endIdx = array.length - 1; endIdx > 0; endIdx--) {
      swap(0, endIdx, array);
      siftDown(0, endIdx - 1, array);
    }
    return array;
  }

  public static void buildMaxHeap(int[] array) {
    int firstParentIdx = (array.length - 2) / 2;
    for (int currentIdx = firstParentIdx; currentIdx >= 0; currentIdx--) {
      siftDown(currentIdx, array.length - 1, array);
    }
  }

  public static void siftDown(int currentIdx, int endIdx, int[] heap) {
    int childOneIdx = currentIdx * 2 + 1;
    while (childOneIdx <= endIdx) {
      int childTwoIdx = currentIdx * 2 + 2 <= endIdx ? currentIdx * 2 + 2 : -1;
      int idxToSwap;
      if (childTwoIdx != -1 && heap[childTwoIdx] > heap[childOneIdx]) {
        idxToSwap = childTwoIdx;
      } else {
        idxToSwap = childOneIdx;
      }
      if (heap[idxToSwap] > heap[currentIdx]) {
        swap(currentIdx, idxToSwap, heap);
        currentIdx = idxToSwap;
        childOneIdx = currentIdx * 2 + 1;
      } else {
        return;
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
