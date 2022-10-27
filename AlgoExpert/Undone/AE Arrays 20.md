# AlgoExpert

## Arrays 20 Apartment Hunting

### Question

You're looking to move into a new apartment on specific street, and you're given a list of contiguous blocks on that street where each block contains an apartment that you could move into.

You also have a list of requirements: a list of buildings that are important to you. For instance, you might value having a school and a gym near your apartment. The list of blocks that you have contains information at every block about all of the buildings that are present and absent at the block in question. For instance, for every block, you might know whether a school, a pool, an office, and a gym are present.

In order to optimize your life, you want to pick an apartment block such that you minimize the farthest distance you'd have to walk from your apartment to reach any of your required buildings.

Write a function that takes in a list of contiguous blocks on a specific street and a list of your required buildings and that returns the location (the index) of the block that's most optimal for you.

If there are multiple most optimal blocks, your function can return the index of any one of them.

### Sample Input

blocks = [
  {
    "gym": false,
    "school": true,
    "store": false,
  },
  {
    "gym": true,
    "school": false,
    "store": false,
  },
  {
    "gym": true,
    "school": true,
    "store": false,
  },
  {
    "gym": false,
    "school": true,
    "store": false,
  },
  {
    "gym": false,
    "school": true,
    "store": true,
  },
]
reqs = ["gym", "school", "store"]

### Sample Output

3 // at index 3, the farthest you'd have to walk to reach a gym, a school, or a store is 1 block; at any other index, you'd have to walk farther

### Optimal Space & Time Complexity

O(br) time | O(br) space - where b is the number of blocks and r is the number of requirements

%

### Key Point

### Solution 1

```java
class Program {
  // O(b^2*r) time | O(b) space - where b is the number of blocks and r is the number of
  // requirements
  public static int apartmentHunting(List<Map<String, Boolean>> blocks, String[] reqs) {
    int[] maxDistancesAtBlocks = new int[blocks.size()];
    Arrays.fill(maxDistancesAtBlocks, Integer.MIN_VALUE);

    for (int i = 0; i < blocks.size(); i++) {
      for (String req : reqs) {
        int closestReqDistance = Integer.MAX_VALUE;
        for (int j = 0; j < blocks.size(); j++) {
          if (blocks.get(j).get(req)) {
            closestReqDistance = Math.min(closestReqDistance, distanceBetween(i, j));
          }
        }
        maxDistancesAtBlocks[i] = Math.max(maxDistancesAtBlocks[i], closestReqDistance);
      }
    }
    return getIdxAtMinValue(maxDistancesAtBlocks);
  }

  public static int getIdxAtMinValue(int[] array) {
    int idxAtMinValue = 0;
    int minValue = Integer.MAX_VALUE;
    for (int i = 0; i < array.length; i++) {
      int currentValue = array[i];
      if (currentValue < minValue) {
        minValue = currentValue;
        idxAtMinValue = i;
      }
    }
    return idxAtMinValue;
  }

  public static int distanceBetween(int a, int b) {
    return Math.abs(a - b);
  }
}

```

### Solution 2

```java
class Program {
  // O(br) time | O(br) space - where b is the number of blocks and r is the number of
  // requirements
  public static int apartmentHunting(List<Map<String, Boolean>> blocks, String[] reqs) {
    int[][] minDistancesFromBlocks = new int[reqs.length][];
    for (int i = 0; i < reqs.length; i++) {
      minDistancesFromBlocks[i] = getMinDistances(blocks, reqs[i]);
    }
    int[] maxDistancesAtBlocks = getMaxDistancesAtBlocks(blocks, minDistancesFromBlocks);
    return getIdxAtMinValue(maxDistancesAtBlocks);
  }

  public static int[] getMinDistances(List<Map<String, Boolean>> blocks, String req) {
    int[] minDistances = new int[blocks.size()];
    int closestReqIdx = Integer.MAX_VALUE;
    for (int i = 0; i < blocks.size(); i++) {
      if (blocks.get(i).get(req)) closestReqIdx = i;
      minDistances[i] = distanceBetween(i, closestReqIdx);
    }
    for (int i = blocks.size() - 1; i >= 0; i--) {
      if (blocks.get(i).get(req)) closestReqIdx = i;
      minDistances[i] = Math.min(minDistances[i], distanceBetween(i, closestReqIdx));
    }
    return minDistances;
  }

  public static int[] getMaxDistancesAtBlocks(
      List<Map<String, Boolean>> blocks, int[][] minDistancesFromBlocks) {
    int[] maxDistancesAtBlocks = new int[blocks.size()];
    for (int i = 0; i < blocks.size(); i++) {
      int[] minDistancesAtBlock = new int[minDistancesFromBlocks.length];
      for (int j = 0; j < minDistancesFromBlocks.length; j++) {
        minDistancesAtBlock[j] = minDistancesFromBlocks[j][i];
      }
      maxDistancesAtBlocks[i] = arrayMax(minDistancesAtBlock);
    }
    return maxDistancesAtBlocks;
  }

  public static int getIdxAtMinValue(int[] array) {
    int idxAtMinValue = 0;
    int minValue = Integer.MAX_VALUE;
    for (int i = 0; i < array.length; i++) {
      int currentValue = array[i];
      if (currentValue < minValue) {
        minValue = currentValue;
        idxAtMinValue = i;
      }
    }
    return idxAtMinValue;
  }

  public static int distanceBetween(int a, int b) {
    return Math.abs(a - b);
  }

  public static int arrayMax(int[] array) {
    int max = array[0];
    for (int a : array) {
      if (a > max) {
        max = a;
      }
    }
    return max;
  }
}

```
