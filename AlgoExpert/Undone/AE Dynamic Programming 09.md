# AlgoExpert

## Dynamic Programming 09 Water Area

### Question

You're given an array of non-negative integers where each non-zero integer represents the height of a pillar of width 1. Imagine water being poured over all of the pillars; write a function that returns the surface area of the water trapped between the pillars viewed from the front. Note that spilled water should be ignored.

You can refer to the first three minutes of this question's video explanation for a visual example.

### Sample Input

heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]

### Sample Output

48

// Below is a visual representation of the sample input.
// The dots and vertical lines represent trapped water and pillars, respectively.
// Note that there are 48 dots.
//        |
//        |
//  |.....|
//  |.....|
//  |.....|
//  |..|..|
//  |..|..|
//  |..|..|.....|
//  |..|..|.....|
// _|..|..|..||.|

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the input array
  public static int waterArea(int[] heights) {
    int[] maxes = new int[heights.length];
    int leftMax = 0;
    for (int i = 0; i < heights.length; i++) {
      int height = heights[i];
      maxes[i] = leftMax;
      leftMax = Math.max(leftMax, height);
    }
    int rightMax = 0;
    for (int i = heights.length - 1; i >= 0; i--) {
      int height = heights[i];
      int minHeight = Math.min(rightMax, maxes[i]);
      if (height < minHeight) {
        maxes[i] = minHeight - height;
      } else {
        maxes[i] = 0;
      }
      rightMax = Math.max(rightMax, height);
    }
    int total = 0;
    for (int i = 0; i < heights.length; i++) {
      total += maxes[i];
    }
    return total;
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the input array
  public static int waterArea(int[] heights) {
    if (heights.length == 0) return 0;

    var leftIdx = 0;
    var rightIdx = heights.length - 1;
    var leftMax = heights[leftIdx];
    var rightMax = heights[rightIdx];
    var surfaceArea = 0;

    while (leftIdx < rightIdx) {
      if (heights[leftIdx] < heights[rightIdx]) {
        leftIdx++;
        leftMax = Math.max(leftMax, heights[leftIdx]);
        surfaceArea += leftMax - heights[leftIdx];
      } else {
        rightIdx--;
        rightMax = Math.max(rightMax, heights[rightIdx]);
        surfaceArea += rightMax - heights[rightIdx];
      }
    }

    return surfaceArea;
  }
}

```
