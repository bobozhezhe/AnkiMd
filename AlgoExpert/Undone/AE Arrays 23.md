# AlgoExpert

## Arrays 23 Minimum Area Rectangle

### Question

You're given an array of points plotted on a 2D graph (the xy-plane). Write a function that returns the minimum area of any rectangle that can be formed using any 4 of these points such that the rectangle's sides are parallel to the x and y axes (i.e., only rectangles with horizontal and vertical sides should be considered--no rectangles with diagonal sides). If no rectangle can be formed, your function should return 0.

The input array will contain points represented by arrays of two integers [x, y]. The input array will never contain duplicate points.

### Sample Input

points =
[
  [1, 5],
  [5, 1],
  [4, 2],
  [2, 4],
  [2, 2],
  [1, 2],
  [4, 5],
  [2, 5],
  [-1, -2],
]

### Sample Output

3
// The rectangle with corners [1, 5], [2, 5], [1, 2], and [2, 2]
// has the minimum area: 3.

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the number of points

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(n) space - where n is the number of points
  public int minimumAreaRectangle(int[][] points) {
    HashMap<Integer, int[]> columns = initializeColumns(points);
    int minimumAreaFound = Integer.MAX_VALUE;
    HashMap<String, Integer> edgesParallelToYAxis = new HashMap<String, Integer>();

    ArrayList<Integer> sortedColumns = new ArrayList<Integer>(columns.keySet());
    Collections.sort(sortedColumns);

    for (Integer x : sortedColumns) {
      int[] yValuesInCurrentColumn = columns.get(x);
      Arrays.sort(yValuesInCurrentColumn);

      for (int currentIdx = 0; currentIdx < yValuesInCurrentColumn.length; currentIdx++) {
        int y2 = yValuesInCurrentColumn[currentIdx];
        for (int previousIdx = 0; previousIdx < currentIdx; previousIdx++) {
          int y1 = yValuesInCurrentColumn[previousIdx];
          String pointString = String.valueOf(y1) + ":" + String.valueOf(y2);

          if (edgesParallelToYAxis.containsKey(pointString)) {
            int currentArea = (x - edgesParallelToYAxis.get(pointString)) * (y2 - y1);
            minimumAreaFound = Math.min(minimumAreaFound, currentArea);
          }

          edgesParallelToYAxis.put(pointString, x);
        }
      }
    }

    return (minimumAreaFound != Integer.MAX_VALUE) ? minimumAreaFound : 0;
  }

  public HashMap<Integer, int[]> initializeColumns(int[][] points) {

    HashMap<Integer, int[]> columns = new HashMap<Integer, int[]>();

    for (int[] point : points) {
      int x = point[0];
      int y = point[1];

      if (!columns.containsKey(x)) {
        columns.put(x, new int[] {});
      }

      int[] column = columns.get(x);
      int[] newColumn = new int[column.length + 1];
      for (int i = 0; i < column.length; i++) {
        newColumn[i] = column[i];
      }
      newColumn[column.length] = y;
      columns.put(x, newColumn);
    }

    return columns;
  }
}

```

### Solution 2

```java
class Program {

  // O(n^2) time | O(n) space - where n is the number of points
  public int minimumAreaRectangle(int[][] points) {
    HashSet<String> pointSet = createPointSet(points);
    int minimumAreaFound = Integer.MAX_VALUE;

    for (int currentIdx = 0; currentIdx < points.length; currentIdx++) {
      int p2x = points[currentIdx][0];
      int p2y = points[currentIdx][1];

      for (int previousIdx = 0; previousIdx < currentIdx; previousIdx++) {
        int p1x = points[previousIdx][0];
        int p1y = points[previousIdx][1];
        boolean pointsShareValue = p1x == p2x || p1y == p2y;

        if (pointsShareValue) continue;

        // If (p1x, p2y) and (p2x, p1y), exist we've found a rectangle.
        boolean point1OnOppositeDiagonalExists = pointSet.contains(convertPointToString(p1x, p2y));
        boolean point2OnOppositeDiagonalExists = pointSet.contains(convertPointToString(p2x, p1y));
        boolean oppositeDiagonalExists =
            point1OnOppositeDiagonalExists && point2OnOppositeDiagonalExists;

        if (oppositeDiagonalExists) {
          int currentArea = Math.abs(p2x - p1x) * Math.abs(p2y - p1y);
          minimumAreaFound = Math.min(minimumAreaFound, currentArea);
        }
      }
    }

    return (minimumAreaFound != Integer.MAX_VALUE) ? minimumAreaFound : 0;
  }

  public String convertPointToString(int x, int y) {
    return String.valueOf(x) + ":" + String.valueOf(y);
  }

  public HashSet<String> createPointSet(int[][] points) {
    HashSet<String> pointSet = new HashSet<String>();
    for (int[] point : points) {
      int x = point[0];
      int y = point[1];
      String pointString = convertPointToString(x, y);
      pointSet.add(pointString);
    }

    return pointSet;
  }
}

```
