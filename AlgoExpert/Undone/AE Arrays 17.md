# AlgoExpert

## Arrays 17 Largest Range

### Question

Write a function that takes in an array of integers and returns an array of length 2 representing the largest range of integers contained in that array.

The first number in the output array should be the first number in the range, while the second number should be the last number in the range.

A range of numbers is defined as a set of numbers that come right after each other in the set of real integers. For instance, the output array [2, 6] represents the range {2, 3, 4, 5, 6}, which is a range of length 5. Note that numbers don't need to be sorted or adjacent in the input array in order to form a range.

You can assume that there will only be one largest range.

### Sample Input

array = [1, 11, 3, 0, 15, 5, 2, 4, 10, 7, 12, 6]

### Sample Output

[0, 7]

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space
  public static int[] largestRange(int[] array) {
    int[] bestRange = new int[2];
    int longestLength = 0;
    Map<Integer, Boolean> nums = new HashMap<Integer, Boolean>();
    for (int num : array) {
      nums.put(num, true);
    }
    for (int num : array) {
      if (!nums.get(num)) {
        continue;
      }
      nums.put(num, false);
      int currentLength = 1;
      int left = num - 1;
      int right = num + 1;
      while (nums.containsKey(left)) {
        nums.put(left, false);
        currentLength++;
        left--;
      }
      while (nums.containsKey(right)) {
        nums.put(right, false);
        currentLength++;
        right++;
      }
      if (currentLength > longestLength) {
        longestLength = currentLength;
        bestRange = new int[] {left + 1, right - 1};
      }
    }
    return bestRange;
  }
}

```
