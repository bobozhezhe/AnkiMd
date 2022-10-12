# AlgoExpert

## Sorting 08 Merge Sort

### Question

Write a function that takes in an array of integers and returns a sorted version of that array. Use the Merge Sort algorithm to sort the array.

If you're unfamiliar with Merge Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

array = [8, 5, 2, 9, 5, 6, 3]

### Sample Output

[2, 3, 5, 5, 6, 8, 9]

### Optimal Space & Time Complexity

Best: O(nlog(n)) time | O(n) space - where n is the length of the input array Average: O(nlog(n)) time | O(n) space - where n is the length of the input array Worst: O(nlog(n)) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // Best: O(nlog(n)) time | O(nlog(n)) space
  // Average: O(nlog(n)) time | O(nlog(n)) space
  // Worst: O(nlog(n)) time | O(nlog(n)) space
  public static int[] mergeSort(int[] array) {
    if (array.length <= 1) {
      return array;
    }
    int middleIdx = array.length / 2;
    int[] leftHalf = Arrays.copyOfRange(array, 0, middleIdx);
    int[] rightHalf = Arrays.copyOfRange(array, middleIdx, array.length);
    return mergeSortedArrays(mergeSort(leftHalf), mergeSort(rightHalf));
  }

  public static int[] mergeSortedArrays(int[] leftHalf, int[] rightHalf) {
    int[] sortedArray = new int[leftHalf.length + rightHalf.length];
    int k = 0;
    int i = 0;
    int j = 0;
    while (i < leftHalf.length && j < rightHalf.length) {
      if (leftHalf[i] <= rightHalf[j]) {
        sortedArray[k++] = leftHalf[i++];
      } else {
        sortedArray[k++] = rightHalf[j++];
      }
    }
    while (i < leftHalf.length) {
      sortedArray[k++] = leftHalf[i++];
    }
    while (j < rightHalf.length) {
      sortedArray[k++] = rightHalf[j++];
    }
    return sortedArray;
  }
}

```

### Solution 2

```java
class Program {
  // Best: O(nlog(n)) time | O(n) space
  // Average: O(nlog(n)) time | O(n) space
  // Worst: O(nlog(n)) time | O(n) space
  public static int[] mergeSort(int[] array) {
    if (array.length <= 1) {
      return array;
    }
    int[] auxiliaryArray = array.clone();
    mergeSort(array, 0, array.length - 1, auxiliaryArray);
    return array;
  }

  public static void mergeSort(int[] mainArray, int startIdx, int endIdx, int[] auxiliaryArray) {
    if (startIdx == endIdx) {
      return;
    }
    int middleIdx = (startIdx + endIdx) / 2;
    mergeSort(auxiliaryArray, startIdx, middleIdx, mainArray);
    mergeSort(auxiliaryArray, middleIdx + 1, endIdx, mainArray);
    doMerge(mainArray, startIdx, middleIdx, endIdx, auxiliaryArray);
  }

  public static void doMerge(
      int[] mainArray, int startIdx, int middleIdx, int endIdx, int[] auxiliaryArray) {
    int k = startIdx;
    int i = startIdx;
    int j = middleIdx + 1;
    while (i <= middleIdx && j <= endIdx) {
      if (auxiliaryArray[i] <= auxiliaryArray[j]) {
        mainArray[k++] = auxiliaryArray[i++];
      } else {
        mainArray[k++] = auxiliaryArray[j++];
      }
    }
    while (i <= middleIdx) {
      mainArray[k++] = auxiliaryArray[i++];
    }
    while (j <= endIdx) {
      mainArray[k++] = auxiliaryArray[j++];
    }
  }
}

```
