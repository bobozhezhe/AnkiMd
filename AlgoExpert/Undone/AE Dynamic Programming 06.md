# AlgoExpert

## Dynamic Programming 06 Max Sum Increasing Subsequenc

### Question

Write a function that takes in a non-empty array of integers and returns the greatest sum that can be generated from a strictly-increasing subsequence in the array as well as an array of the numbers in that subsequence.

A subsequence of an array is a set of numbers that aren't necessarily adjacent in the array but that are in the same order as they appear in the array. For instance, the numbers [1, 3, 4] form a subsequence of the array [1, 2, 3, 4], and so do the numbers [2, 4]. Note that a single number in an array and the array itself are both valid subsequences of the array.

You can assume that there will only be one increasing subsequence with the greatest sum.

### Sample Input

array = [10, 70, 20, 30, 50, 11, 30]

### Sample Output

[110, [10, 20, 30, 50]] // The subsequence [10, 20, 30, 50] is strictly increasing and yields the greatest sum: 110.

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static List<List<Integer>> maxSumIncreasingSubsequence(int[] array) {
    int[] sequences = new int[array.length];
    Arrays.fill(sequences, Integer.MIN_VALUE);
    int[] sums = array.clone();
    int maxSumIdx = 0;
    for (int i = 0; i < array.length; i++) {
      int currentNum = array[i];
      for (int j = 0; j < i; j++) {
        int otherNum = array[j];
        if (otherNum < currentNum && sums[j] + currentNum >= sums[i]) {
          sums[i] = sums[j] + currentNum;
          sequences[i] = j;
        }
      }
      if (sums[i] >= sums[maxSumIdx]) {
        maxSumIdx = i;
      }
    }
    return buildSequence(array, sequences, maxSumIdx, sums[maxSumIdx]);
  }

  public static List<List<Integer>> buildSequence(
      int[] array, int[] sequences, int currentIdx, int sums) {
    List<List<Integer>> sequence = new ArrayList<List<Integer>>();
    sequence.add(new ArrayList<Integer>());
    sequence.add(new ArrayList<Integer>());
    sequence.get(0).add(sums);
    while (currentIdx != Integer.MIN_VALUE) {
      sequence.get(1).add(0, array[currentIdx]);
      currentIdx = sequences[currentIdx];
    }
    return sequence;
  }
}
```
