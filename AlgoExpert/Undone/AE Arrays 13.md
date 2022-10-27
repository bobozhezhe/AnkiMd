# AlgoExpert

## Arrays 13 First Duplicate Value

### Question

Given an array of integers between 1 and n, inclusive, where n is the length of the array, write a function that returns the first integer that appears more than once (when the array is read from left to right).

In other words, out of all the integers that might occur more than once in the input array, your function should return the one whose first duplicate value has the minimum index.

If no integer appears more than once, your function should return -1.

Note that you're allowed to mutate the input array.

### Sample Input #1

array = [2, 1, 5, 2, 3, 3, 4]

### Sample Output #1

2 // 2 is the first integer that appears more than once.
// 3 also appears more than once, but the second 3 appears after the second 2.

### Sample Input #2

array = [2, 1, 5, 3, 3, 2, 4]

### Sample Output #2

3 // 3 is the first integer that appears more than once.
// 2 also appears more than once, but the second 2 appears after the second 3.

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(1) space - where n is the length of the input array
  public int firstDuplicateValue(int[] array) {
    int minimumSecondIndex = array.length;
    for (int i = 0; i < array.length; i++) {
      int value = array[i];
      for (int j = i + 1; j < array.length; j++) {
        int valueToCompare = array[j];
        if (value == valueToCompare) {
          minimumSecondIndex = Math.min(minimumSecondIndex, j);
        }
      }
    }

    if (minimumSecondIndex == array.length) {
      return -1;
    }

    return array[minimumSecondIndex];
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(n) space - where n is the length of the input array
  public int firstDuplicateValue(int[] array) {
    HashSet<Integer> seen = new HashSet<Integer>();
    for (int value : array) {
      if (seen.contains(value)) return value;
      seen.add(value);
    }
    return -1;
  }
}

```

### Solution 3

```java
class Program {

  // O(n) time | O(1) space - where n is the length of the input array
  public int firstDuplicateValue(int[] array) {
    for (int value : array) {
      int absValue = Math.abs(value);
      if (array[absValue - 1] < 0) return absValue;
      array[absValue - 1] *= -1;
    }
    return -1;
  }
}

```
