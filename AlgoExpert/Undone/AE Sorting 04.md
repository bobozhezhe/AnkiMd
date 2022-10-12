# AlgoExpert

## Sorting 04 Three Number Sort

### Question

You're given an array of integers and another array of three distinct integers. The first array is guaranteed to only contain integers that are in the second array, and the second array represents a desired order for the integers in the first array. For example, a second array of [x, y, z] represents a desired order of [x, x, ..., x, y, y, ..., y, z, z, ..., z] in the first array.

Write a function that sorts the first array according to the desired order in the second array.

The function should perform this in place (i.e., it should mutate the input array), and it shouldn't use any auxiliary space (i.e., it should run with constant space: O(1) space).

Note that the desired order won't necessarily be ascending or descending and that the first array won't necessarily contain all three integers found in the second array—it might only contain one or two.

### Sample Input

array = [1, 0, 0, -1, -1, 0, 1, 1]
order = [0, 1, -1]

### Sample Output

[0, 0, 0, 1, 1, 1, -1, -1]

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the array
  public int[] threeNumberSort(int[] array, int[] order) {
    int[] valueCounts = new int[] {0, 0, 0};

    for (int element : array) {
      int orderIdx = getIndex(order, element);
      valueCounts[orderIdx] += 1;
    }

    for (int i = 0; i < 3; i++) {
      int value = order[i];
      int count = valueCounts[i];

      int numElementsBefore = getSum(valueCounts, i);
      for (int n = 0; n < count; n++) {
        int currentIdx = numElementsBefore + n;
        array[currentIdx] = value;
      }
    }

    return array;
  }

  public int getIndex(int[] array, int element) {
    for (int i = 0; i < array.length; i++) {
      if (array[i] == element) {
        return i;
      }
    }
    return -1;
  }

  public int getSum(int[] array, int end) {
    int sum = 0;
    for (int i = 0; i < end; i++) sum += array[i];
    return sum;
  }
}

```

### Solution 2

```java
lass Program {
  // O(n) time | O(1) space - where n is the length of the array
  public int[] threeNumberSort(int[] array, int[] order) {
    int firstValue = order[0];
    int thirdValue = order[2];

    int firstIdx = 0;
    for (int idx = 0; idx < array.length; idx++) {
      if (array[idx] == firstValue) {
        swap(firstIdx, idx, array);
        firstIdx += 1;
      }
    }

    int thirdIdx = array.length - 1;
    for (int idx = array.length - 1; idx >= 0; idx--) {
      if (array[idx] == thirdValue) {
        swap(thirdIdx, idx, array);
        thirdIdx -= 1;
      }
    }

    return array;
  }

  public void swap(int i, int j, int[] array) {
    int temp = array[j];
    array[j] = array[i];
    array[i] = temp;
  }
}

```

### Solution 3

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the array
  public int[] threeNumberSort(int[] array, int[] order) {
    int firstValue = order[0];
    int secondValue = order[1];

    // Keep track of the indices where the values are stored
    int firstIdx = 0;
    int secondIdx = 0;
    int thirdIdx = array.length - 1;

    while (secondIdx <= thirdIdx) {
      int value = array[secondIdx];

      if (value == firstValue) {
        swap(firstIdx, secondIdx, array);
        firstIdx += 1;
        secondIdx += 1;
      } else if (value == secondValue) {
        secondIdx += 1;
      } else {
        swap(secondIdx, thirdIdx, array);
        thirdIdx -= 1;
      }
    }

    return array;
  }

  public void swap(int i, int j, int[] array) {
    int temp = array[j];
    array[j] = array[i];
    array[i] = temp;
  }
}

```
