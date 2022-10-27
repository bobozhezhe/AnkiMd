# AlgoExpert

## Arrays 24 Line Through Points

### Question

You're given an array of points plotted on a 2D graph (the xy-plane). Write a function that returns the maximum number of points that a single line (or potentially multiple lines) on the graph passes through.

The input array will contain points represented by an array of two integers [x, y]. The input array will never contain duplicate points and will always contain at least one point.

### Sample Input

points = [
  [1, 1],
  [2, 2],
  [3, 3],
  [0, 4],
  [-2, 6],
  [4, 0],
  [2, 1],
]

### Sample Output

4 // A line passes through points: [-2, 6], [0, 4], [2, 2], [4, 0].

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the number of points

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(n) space - where n is the number of points
  public int lineThroughPoints(int[][] points) {
    int maxNumberOfPointsOnLine = 1;

    for (int idx1 = 0; idx1 < points.length; idx1++) {
      int[] p1 = points[idx1];
      HashMap<String, Integer> slopes = new HashMap<String, Integer>();

      for (int idx2 = idx1 + 1; idx2 < points.length; idx2++) {
        int[] p2 = points[idx2];
        int[] slopeOfLineBetweenPoints = getSlopeOfLineBetweenPoints(p1, p2);
        int rise = slopeOfLineBetweenPoints[0];
        int run = slopeOfLineBetweenPoints[1];

        String slopeKey = createHashableKeyForRational(rise, run);
        if (!slopes.containsKey(slopeKey)) {
          slopes.put(slopeKey, 1);
        }
        slopes.put(slopeKey, slopes.get(slopeKey) + 1);
      }

      int currentMaxNumberOfPointsOnLine = maxSlope(slopes);
      maxNumberOfPointsOnLine = Math.max(maxNumberOfPointsOnLine, currentMaxNumberOfPointsOnLine);
    }

    return maxNumberOfPointsOnLine;
  }

  public int[] getSlopeOfLineBetweenPoints(int[] p1, int[] p2) {
    int p1x = p1[0];
    int p1y = p1[1];
    int p2x = p2[0];
    int p2y = p2[1];

    int[] slope = new int[] {1, 0}; // slope of a vertical line

    if (p1x != p2x) { // if line is not vertical
      int xDiff = p1x - p2x;
      int yDiff = p1y - p2y;
      int gcd = getGreatestCommonDivisor(Math.abs(xDiff), Math.abs(yDiff));
      xDiff = xDiff / gcd;
      yDiff = yDiff / gcd;
      if (xDiff < 0) {
        xDiff *= -1;
        yDiff *= -1;
      }

      slope = new int[] {yDiff, xDiff};
    }

    return slope;
  }

  public String createHashableKeyForRational(int numerator, int denominator) {
    return String.valueOf(numerator) + ":" + String.valueOf(denominator);
  }

  public int maxSlope(HashMap<String, Integer> slopes) {
    int currentMax = 0;
    for (Map.Entry<String, Integer> slope : slopes.entrySet()) {
      currentMax = Math.max(slope.getValue(), currentMax);
    }
    return currentMax;
  }

  public int getGreatestCommonDivisor(int num1, int num2) {
    int a = num1;
    int b = num2;
    while (true) {
      if (a == 0) {
        return b;
      }
      if (b == 0) {
        return a;
      }
      int temp = a;
      a = b;
      b = temp % b;
    }
  }
}

```
