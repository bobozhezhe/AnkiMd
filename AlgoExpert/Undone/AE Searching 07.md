# AlgoExpert

## Searching 07 Index Equals Value

### Question

Write a function that takes in a sorted array of distinct integers and returns the first index in the array that is equal to the value at that index. In other words, your function should return the minimum index where index == array[index].

If there is no such index, your function should return -1.

### Sample Input

array = [-5, -3, 0, 3, 4, 5, 9]

### Sample Output

3 // 3 == array[3]

### Optimal Space & Time Complexity

O(log(n)) time | O(1) space - where n is the length of the input array
%

### Key Point

### Solution 1

```java
class Program {

  // O(n) time | O(1) space - where n is the length of the input array
  public int indexEqualsValue(int[] array) {
    for (int index = 0; index < array.length; index++) {
      int value = array[index];
      if (index == value) {
        return index;
      }
    }

    return -1;
  }
}

```

### Solution 2

```java
class Program {

  // O(log(n)) time | O(log(n)) space - where n is the length of the input array
  public int indexEqualsValue(int[] array) {
    return indexEqualsValueHelper(array, 0, array.length - 1);
  }

  public int indexEqualsValueHelper(int[] array, int leftIndex, int rightIndex) {
    if (leftIndex > rightIndex) {
      return -1;
    }

    int middleIndex = leftIndex + (rightIndex - leftIndex) / 2;
    int middleValue = array[middleIndex];

    if (middleValue < middleIndex) {
      return indexEqualsValueHelper(array, middleIndex + 1, rightIndex);
    } else if ((middleValue == middleIndex) && (middleIndex == 0)) {
      return middleIndex;
    } else if ((middleValue == middleIndex) && (array[middleIndex - 1] < (middleIndex - 1))) {
      return middleIndex;
    } else {
      return indexEqualsValueHelper(array, leftIndex, middleIndex - 1);
    }
  }
}

```

### Solution 3

```java
class Program {

  // O(log(n)) time | O(1) space - where n is the length of the input array
  public int indexEqualsValue(int[] array) {
    int leftIndex = 0;
    int rightIndex = array.length - 1;

    while (leftIndex <= rightIndex) {
      int middleIndex = rightIndex + (leftIndex - rightIndex) / 2;
      int middleValue = array[middleIndex];

      if (middleValue < middleIndex) {
        leftIndex = middleIndex + 1;
      } else if ((middleValue == middleIndex) && (middleIndex == 0)) {
        return middleIndex;
      } else if ((middleValue == middleIndex) && (array[middleIndex - 1] < (middleIndex - 1))) {
        return middleIndex;
      } else {
        rightIndex = middleIndex - 1;
      }
    }

    return -1;
  }
}

```
