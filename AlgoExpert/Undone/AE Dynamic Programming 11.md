# AlgoExpert

## Dynamic Programming 11 Disk Stacking

### Question

You're given a non-empty array of arrays where each subarray holds three integers and represents a disk. These integers denote each disk's width, depth, and height, respectively. Your goal is to stack up the disks and to maximize the total height of the stack. A disk must have a strictly smaller width, depth, and height than any other disk below it.

Write a function that returns an array of the disks in the final stack, starting with the top disk and ending with the bottom disk. Note that you can't rotate disks; in other words, the integers in each subarray must represent [width, depth, height] at all times.

You can assume that there will only be one stack with the greatest total height.

### Sample Input

disks = [[2, 1, 2], [3, 2, 3], [2, 2, 8], [2, 3, 4], [1, 3, 1], [4, 4, 5]]

### Sample Output

[[2, 1, 2], [3, 2, 3], [4, 4, 5]]
// 10 (2 + 3 + 5) is the tallest height we can get by
// stacking disks following the rules laid out above.

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the number of disks

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static List<Integer[]> diskStacking(List<Integer[]> disks) {
    disks.sort((disk1, disk2) -> disk1[2].compareTo(disk2[2]));
    int[] heights = new int[disks.size()];
    for (int i = 0; i < disks.size(); i++) {
      heights[i] = disks.get(i)[2];
    }
    int[] sequences = new int[disks.size()];
    for (int i = 0; i < disks.size(); i++) {
      sequences[i] = Integer.MIN_VALUE;
    }
    int maxHeightIdx = 0;
    for (int i = 1; i < disks.size(); i++) {
      Integer[] currentDisk = disks.get(i);
      for (int j = 0; j < i; j++) {
        Integer[] otherDisk = disks.get(j);
        if (areValidDimensions(otherDisk, currentDisk)) {
          if (heights[i] <= currentDisk[2] + heights[j]) {
            heights[i] = currentDisk[2] + heights[j];
            sequences[i] = j;
          }
        }
      }
      if (heights[i] >= heights[maxHeightIdx]) {
        maxHeightIdx = i;
      }
    }
    return buildSequence(disks, sequences, maxHeightIdx);
  }

  public static boolean areValidDimensions(Integer[] o, Integer[] c) {
    return o[0] < c[0] && o[1] < c[1] && o[2] < c[2];
  }

  public static List<Integer[]> buildSequence(
      List<Integer[]> array, int[] sequences, int currentIdx) {
    List<Integer[]> sequence = new ArrayList<Integer[]>();
    while (currentIdx != Integer.MIN_VALUE) {
      sequence.add(0, array.get(currentIdx));
      currentIdx = sequences[currentIdx];
    }
    return sequence;
  }
}

```
