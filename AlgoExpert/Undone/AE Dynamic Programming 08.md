# AlgoExpert

## Dynamic Programming 08 Min Number Of Jumps

### Question

You're given a non-empty array of positive integers where each integer represents the maximum number of steps you can take forward in the array. For example, if the element at index 1 is 3, you can go from index 1 to index 2, 3, or 4.

Write a function that returns the minimum number of jumps needed to reach the final index.

Note that jumping from index i to index i + x always constitutes one jump, no matter how large x is.

### Sample Input

array = [3, 4, 2, 1, 2, 3, 7, 1, 1, 1, 3]

### Sample Output

4 // 3 --> (4 or 2) --> (2 or 3) --> 7 --> 3

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static int minNumberOfJumps(int[] array) {
    int[] jumps = new int[array.length];
    Arrays.fill(jumps, Integer.MAX_VALUE);
    jumps[0] = 0;
    for (int i = 1; i < array.length; i++) {
      for (int j = 0; j < i; j++) {
        if (array[j] >= i - j) {
          jumps[i] = Math.min(jumps[j] + 1, jumps[i]);
        }
      }
    }
    return jumps[jumps.length - 1];
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(1) space
  public static int minNumberOfJumps(int[] array) {
    if (array.length == 1) {
      return 0;
    }
    int jumps = 0;
    int maxReach = array[0];
    int steps = array[0];
    for (int i = 1; i < array.length - 1; i++) {
      maxReach = Math.max(maxReach, i + array[i]);
      steps--;
      if (steps == 0) {
        jumps++;
        steps = maxReach - i;
      }
    }
    return jumps + 1;
  }
}
```
