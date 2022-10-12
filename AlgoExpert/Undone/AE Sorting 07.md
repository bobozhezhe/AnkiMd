# AlgoExpert

## Sorting 07 Radix Sort

### Question

Write a function that takes in an array of non-negative integers and returns a sorted version of that array. Use the Radix Sort algorithm to sort the array.

If you're unfamiliar with Radix Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

array = [8762, 654, 3008, 345, 87, 65, 234, 12, 2]

### Sample Output

[2, 12, 65, 87, 234, 345, 654, 3008, 8762]

### Optimal Space & Time Complexity

O(d * (n + b)) time | O(n + b) space - where n is the length of the input array, d is the max number of digits, and b is the base of the numbering system used

%

### Key Point

### Solution 1

```java
class Program {

  // O(d * (n + b)) time | O(n + b) space - where n is the length of the input array,
  // d is the max number of digits, and b is the base of the numbering system used
  public ArrayList<Integer> radixSort(ArrayList<Integer> array) {
    if (array.size() == 0) {
      return array;
    }

    int maxNumber = Collections.max(array);

    int digit = 0;
    while ((maxNumber / Math.pow(10, digit)) > 0) {
      countingSort(array, digit);
      digit += 1;
    }

    return array;
  }

  public void countingSort(ArrayList<Integer> array, int digit) {
    int[] sortedArray = new int[array.size()];
    int[] countArray = new int[10];

    int digitColumn = (int) Math.pow(10, digit);
    for (int num : array) {
      int countIndex = (num / digitColumn) % 10;
      countArray[countIndex] += 1;
    }

    for (int idx = 1; idx < 10; idx++) {
      countArray[idx] += countArray[idx - 1];
    }

    for (int idx = array.size() - 1; idx > -1; idx--) {
      int countIndex = (array.get(idx) / digitColumn) % 10;
      countArray[countIndex] -= 1;
      int sortedIndex = countArray[countIndex];
      sortedArray[sortedIndex] = array.get(idx);
    }

    for (int idx = 0; idx < array.size(); idx++) {
      array.set(idx, sortedArray[idx]);
    }
  }
}

```
