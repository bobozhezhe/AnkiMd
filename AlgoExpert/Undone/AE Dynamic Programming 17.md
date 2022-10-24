# AlgoExpert

## Dynamic Programming 17 Longest Increasing Subsequence

### Question

Given a non-empty array of integers, write a function that returns the longest strictly-increasing subsequence in the array.

A subsequence of an array is a set of numbers that aren't necessarily adjacent in the array but that are in the same order as they appear in the array. For instance, the numbers [1, 3, 4] form a subsequence of the array [1, 2, 3, 4], and so do the numbers [2, 4]. Note that a single number in an array and the array itself are both valid subsequences of the array.

You can assume that there will only be one longest increasing subsequence.

### Sample Input

array = [5, 7, -24, 12, 10, 2, 3, 12, 5, 6, 35]

### Sample Output

[-24, 2, 3, 5, 6, 35]

### Optimal Space & Time Complexity

O(nlogn) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static List<Integer> longestIncreasingSubsequence(int[] array) {
    int[] sequences = new int[array.length];
    Arrays.fill(sequences, Integer.MIN_VALUE);
    int[] lengths = new int[array.length];
    Arrays.fill(lengths, 1);
    int maxLengthIdx = 0;
    for (int i = 0; i < array.length; i++) {
      int currentNum = array[i];
      for (int j = 0; j < i; j++) {
        int otherNum = array[j];
        if (otherNum < currentNum && lengths[j] + 1 >= lengths[i]) {
          lengths[i] = lengths[j] + 1;
          sequences[i] = j;
        }
      }
      if (lengths[i] >= lengths[maxLengthIdx]) {
        maxLengthIdx = i;
      }
    }
    return buildSequence(array, sequences, maxLengthIdx);
  }

  public static List<Integer> buildSequence(int[] array, int[] sequences, int currentIdx) {
    List<Integer> sequence = new ArrayList<Integer>();
    while (currentIdx != Integer.MIN_VALUE) {
      sequence.add(0, array[currentIdx]);
      currentIdx = sequences[currentIdx];
    }
    return sequence;
  }
}

```

### Solution 2

```java
class Program {
  // O(nlogn) time | O(n) space
  public static List<Integer> longestIncreasingSubsequence(int[] array) {
    int[] sequences = new int[array.length];
    int[] indices = new int[array.length + 1];
    Arrays.fill(indices, Integer.MIN_VALUE);
    int length = 0;
    for (int i = 0; i < array.length; i++) {
      int num = array[i];
      int newLength = binarySearch(1, length, indices, array, num);
      sequences[i] = indices[newLength - 1];
      indices[newLength] = i;
      length = Math.max(length, newLength);
    }
    return buildSequence(array, sequences, indices[length]);
  }

  public static int binarySearch(int startIdx, int endIdx, int[] indices, int[] array, int num) {
    if (startIdx > endIdx) {
      return startIdx;
    }
    int middleIdx = (startIdx + endIdx) / 2;
    if (array[indices[middleIdx]] < num) {
      startIdx = middleIdx + 1;
    } else {
      endIdx = middleIdx - 1;
    }
    return binarySearch(startIdx, endIdx, indices, array, num);
  }

  public static List<Integer> buildSequence(int[] array, int[] sequences, int currentIdx) {
    List<Integer> sequence = new ArrayList<Integer>();
    while (currentIdx != Integer.MIN_VALUE) {
      sequence.add(0, array[currentIdx]);
      currentIdx = sequences[currentIdx];
    }
    return sequence;
  }
}

```
