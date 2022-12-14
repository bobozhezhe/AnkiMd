# AlgoExpert

## Dynamic Programming 14 Maximize Expression

### Question

Write a function that takes in an array of integers and returns the largest possible value for the expression array[a] - array[b] + array[c] - array[d], where a, b, c, and d are indices of the array and a < b < c < d.

If the input array has fewer than 4 elements, your function should return 0.

### Sample Input

array = [3, 6, 1, -3, 2, 7]

### Sample Output

4
// Choose a = 1, b = 3, c = 4, and d = 5
// -> 6 - (-3) + 2 - 7 = 4

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the array

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^4) time | O(1) space - where n is the length of the array
  public int maximizeExpression(int[] array) {
    if (array.length < 4) {
      return 0;
    }

    int maximumValueFound = Integer.MIN_VALUE;

    for (int a = 0; a < array.length; a++) {
      int aValue = array[a];
      for (int b = a + 1; b < array.length; b++) {
        int bValue = array[b];
        for (int c = b + 1; c < array.length; c++) {
          int cValue = array[c];
          for (int d = c + 1; d < array.length; d++) {
            int dValue = array[d];
            int expressionValue = evaluateExpression(aValue, bValue, cValue, dValue);
            maximumValueFound = Math.max(expressionValue, maximumValueFound);
          }
        }
      }
    }

    return maximumValueFound;
  }

  public int evaluateExpression(int a, int b, int c, int d) {
    return a - b + c - d;
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(n) space - where n is the length of the array
  public int maximizeExpression(int[] array) {
    if (array.length < 4) {
      return 0;
    }

    ArrayList<Integer> maxOfA = new ArrayList<Integer>(Arrays.asList(array[0]));
    ArrayList<Integer> maxOfAMinusB = new ArrayList<Integer>(Arrays.asList(Integer.MIN_VALUE));
    ArrayList<Integer> maxOfAMinusBPlusC =
        new ArrayList<Integer>(Arrays.asList(Integer.MIN_VALUE, Integer.MIN_VALUE));
    ArrayList<Integer> maxOfAMinusBPlusCMinusD =
        new ArrayList<Integer>(
            Arrays.asList(Integer.MIN_VALUE, Integer.MIN_VALUE, Integer.MIN_VALUE));

    for (int idx = 1; idx < array.length; idx++) {
      int currentMax = Math.max(maxOfA.get(idx - 1), array[idx]);
      maxOfA.add(currentMax);
    }

    for (int idx = 1; idx < array.length; idx++) {
      int currentMax = Math.max(maxOfAMinusB.get(idx - 1), maxOfA.get(idx - 1) - array[idx]);
      maxOfAMinusB.add(currentMax);
    }

    for (int idx = 2; idx < array.length; idx++) {
      int currentMax =
          Math.max(maxOfAMinusBPlusC.get(idx - 1), maxOfAMinusB.get(idx - 1) + array[idx]);
      maxOfAMinusBPlusC.add(currentMax);
    }

    for (int idx = 3; idx < array.length; idx++) {
      int currentMax =
          Math.max(
              maxOfAMinusBPlusCMinusD.get(idx - 1), maxOfAMinusBPlusC.get(idx - 1) - array[idx]);
      maxOfAMinusBPlusCMinusD.add(currentMax);
    }

    return maxOfAMinusBPlusCMinusD.get(maxOfAMinusBPlusCMinusD.size() - 1);
  }
}

```
