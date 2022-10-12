# AlgoExpert

## Recursion 06 Staircase Traversal

### Question

You're given two positive integers representing the height of a staircase and the maximum number of steps that you can advance up the staircase at a time. Write a function that returns the number of ways in which you can climb the staircase.

For example, if you were given a staircase of height = 3 and maxSteps = 2 you could climb the staircase in 3 ways. You could take 1 step, 1 step, then 1 step, you could also take 1 step, then 2 steps, and you could take 2 steps, then 1 step.

Note that maxSteps <= height will always be true.

### Sample Input

height = 4
maxSteps = 2

### Sample Output

5
// You can climb the staircase in the following ways:
// 1, 1, 1, 1
// 1, 1, 2
// 1, 2, 1
// 2, 1, 1
// 2, 2

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the height of the staircase

%

### Key Point

### Solution 1

```java
class Program {

  // O(k^n) time | O(n) space - where n is the height of the staircase and k is the number of
  // allowed steps
  public int staircaseTraversal(int height, int maxSteps) {
    return numberOfWaysToTop(height, maxSteps);
  }

  public int numberOfWaysToTop(int height, int maxSteps) {
    if (height <= 1) {
      return 1;
    }

    int numberOfWays = 0;
    for (int step = 1; step < Math.min(maxSteps, height) + 1; step++) {
      numberOfWays += numberOfWaysToTop(height - step, maxSteps);
    }

    return numberOfWays;
  }
}

```

### Solution 2

```java
class Program {

  // O(n * k) time | O(n) space - where n is the height of the staircase and k is the number of
  // allowed steps
  public int staircaseTraversal(int height, int maxSteps) {
    HashMap<Integer, Integer> memoize = new HashMap<Integer, Integer>();
    memoize.put(0, 1);
    memoize.put(1, 1);
    return numberOfWaysToTop(height, maxSteps, memoize);
  }

  public int numberOfWaysToTop(int height, int maxSteps, HashMap<Integer, Integer> memoize) {
    if (memoize.containsKey(height)) {
      return memoize.get(height);
    }

    int numberOfWays = 0;
    for (int step = 1; step < Math.min(maxSteps, height) + 1; step++) {
      numberOfWays += numberOfWaysToTop(height - step, maxSteps, memoize);
    }

    memoize.put(height, numberOfWays);

    return numberOfWays;
  }
}

```

### Solution 3

```java
class Program {

  // O(n * k) time | O(n) space - where n is the height of the staircase and k is the number of
  // allowed steps
  public int staircaseTraversal(int height, int maxSteps) {
    int[] waysToTop = new int[height + 1];
    waysToTop[0] = 1;
    waysToTop[1] = 1;

    for (int currentHeight = 2; currentHeight < height + 1; currentHeight++) {
      int step = 1;
      while (step <= maxSteps && step <= currentHeight) {
        waysToTop[currentHeight] = waysToTop[currentHeight] + waysToTop[currentHeight - step];
        step += 1;
      }
    }

    return waysToTop[height];
  }
}

```

### Solution 4

```java
class Program {

  // O(n) time | O(n) space - where n is the height of the staircase
  public int staircaseTraversal(int height, int maxSteps) {
    int currentNumberOfWays = 0;
    ArrayList<Integer> waysToTop = new ArrayList<Integer>();
    waysToTop.add(1);

    for (int currentHeight = 1; currentHeight < height + 1; currentHeight++) {
      int startOfWindow = currentHeight - maxSteps - 1;
      int endOfWindow = currentHeight - 1;

      if (startOfWindow >= 0) {
        currentNumberOfWays -= waysToTop.get(startOfWindow);
      }

      currentNumberOfWays += waysToTop.get(endOfWindow);
      waysToTop.add(currentNumberOfWays);
    }

    return waysToTop.get(height);
  }
}

```
