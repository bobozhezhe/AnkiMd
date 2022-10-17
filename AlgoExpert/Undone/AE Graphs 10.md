# AlgoExpert

## Graphs 10 Rectangle Mania

### Question

Write a function that takes in a list of Cartesian coordinates (i.e., (x, y) coordinates) and returns the number of rectangles formed by these coordinates.

A rectangle must have its four corners amongst the coordinates in order to be counted, and we only care about rectangles with sides parallel to the x and y axes (i.e., with horizontal and vertical sides--no diagonal sides).

You can also assume that no coordinate will be farther than 100 units from the origin.

### Sample Input

coords = [
  [0, 0], [0, 1], [1, 1], [1, 0],
  [2, 1], [2, 0], [3, 1], [3, 0],
]

### Sample Output

6

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the number of coordinates

%

### Key Point

### Solution 1

```java
class Program {
  static String UP = "up";
  static String RIGHT = "right";
  static String DOWN = "down";
  static String LEFT = "left";

  // O(n^2) time | O(n^2) space - where n is the number of coordinates
  public static int rectangleMania(List<Integer[]> coords) {
    Map<String, Map<String, List<Integer[]>>> coordsTable = getCoordsTable(coords);
    return getRectangleCount(coords, coordsTable);
  }

  public static Map<String, Map<String, List<Integer[]>>> getCoordsTable(List<Integer[]> coords) {
    Map<String, Map<String, List<Integer[]>>> coordsTable =
        new HashMap<String, Map<String, List<Integer[]>>>();
    for (Integer[] coord1 : coords) {
      Map<String, List<Integer[]>> coord1Directions = new HashMap<String, List<Integer[]>>();
      coord1Directions.put(UP, new ArrayList<Integer[]>());
      coord1Directions.put(RIGHT, new ArrayList<Integer[]>());
      coord1Directions.put(DOWN, new ArrayList<Integer[]>());
      coord1Directions.put(LEFT, new ArrayList<Integer[]>());
      for (Integer[] coord2 : coords) {
        String coord2Direction = getCoordDirection(coord1, coord2);
        if (coord1Directions.containsKey(coord2Direction))
          coord1Directions.get(coord2Direction).add(coord2);
      }
      String coord1String = coordToString(coord1);
      coordsTable.put(coord1String, coord1Directions);
    }
    return coordsTable;
  }

  public static String getCoordDirection(Integer[] coord1, Integer[] coord2) {
    if (coord2[1] == coord1[1]) {
      if (coord2[0] > coord1[0]) {
        return RIGHT;
      } else if (coord2[0] < coord1[0]) {
        return LEFT;
      }
    } else if (coord2[0] == coord1[0]) {
      if (coord2[1] > coord1[1]) {
        return UP;
      } else if (coord2[1] < coord1[1]) {
        return DOWN;
      }
    }
    return "";
  }

  public static int getRectangleCount(
      List<Integer[]> coords, Map<String, Map<String, List<Integer[]>>> coordsTable) {
    int rectangleCount = 0;
    for (Integer[] coord : coords) {
      rectangleCount += clockwiseCountRectangles(coord, coordsTable, UP, coord);
    }
    return rectangleCount;
  }

  public static int clockwiseCountRectangles(
      Integer[] coord,
      Map<String, Map<String, List<Integer[]>>> coordsTable,
      String direction,
      Integer[] origin) {
    String coordString = coordToString(coord);
    if (direction == LEFT) {
      boolean rectangleFound = coordsTable.get(coordString).get(LEFT).contains(origin);
      return rectangleFound ? 1 : 0;
    } else {
      int rectangleCount = 0;
      String nextDirection = getNextClockwiseDirection(direction);
      for (Integer[] nextCoord : coordsTable.get(coordString).get(direction)) {
        rectangleCount += clockwiseCountRectangles(nextCoord, coordsTable, nextDirection, origin);
      }
      return rectangleCount;
    }
  }

  public static String getNextClockwiseDirection(String direction) {
    if (direction == UP) return RIGHT;
    if (direction == RIGHT) return DOWN;
    if (direction == DOWN) return LEFT;
    return "";
  }

  public static String coordToString(Integer[] coord) {
    return Integer.toString(coord[0]) + "-" + Integer.toString(coord[1]);
  }
}

```

### Solution 2

```java
class Program {
  static String UP = "up";
  static String RIGHT = "right";
  static String DOWN = "down";

  // O(n^2) time | O(n) space - where n is the number of coordinates
  public static int rectangleMania(List<Integer[]> coords) {
    Map<String, Map<Integer, List<Integer[]>>> coordsTable = getCoordsTable(coords);
    return getRectangleCount(coords, coordsTable);
  }

  public static Map<String, Map<Integer, List<Integer[]>>> getCoordsTable(List<Integer[]> coords) {
    Map<String, Map<Integer, List<Integer[]>>> coordsTable =
        new HashMap<String, Map<Integer, List<Integer[]>>>();
    coordsTable.put("x", new HashMap<Integer, List<Integer[]>>());
    coordsTable.put("y", new HashMap<Integer, List<Integer[]>>());
    for (Integer[] coord : coords) {
      if (!coordsTable.get("x").containsKey(coord[0])) {
        coordsTable.get("x").put(coord[0], new ArrayList<Integer[]>());
      }
      if (!coordsTable.get("y").containsKey(coord[1])) {
        coordsTable.get("y").put(coord[1], new ArrayList<Integer[]>());
      }
      coordsTable.get("x").get(coord[0]).add(coord);
      coordsTable.get("y").get(coord[1]).add(coord);
    }
    return coordsTable;
  }

  public static int getRectangleCount(
      List<Integer[]> coords, Map<String, Map<Integer, List<Integer[]>>> coordsTable) {
    int rectangleCount = 0;
    for (Integer[] coord : coords) {
      int lowerLeftY = coord[1];
      rectangleCount += clockwiseCountRectangles(coord, coordsTable, UP, lowerLeftY);
    }
    return rectangleCount;
  }

  public static int clockwiseCountRectangles(
      Integer[] coord1,
      Map<String, Map<Integer, List<Integer[]>>> coordsTable,
      String direction,
      int lowerLeftY) {
    if (direction == DOWN) {
      List<Integer[]> relevantCoords = coordsTable.get("x").get(coord1[0]);
      for (Integer[] coord2 : relevantCoords) {
        int lowerRightY = coord2[1];
        if (lowerRightY == lowerLeftY) return 1;
      }
      return 0;
    } else {
      int rectangleCount = 0;
      if (direction == UP) {
        List<Integer[]> relevantCoords = coordsTable.get("x").get(coord1[0]);
        for (Integer[] coord2 : relevantCoords) {
          boolean isAbove = coord2[1] > coord1[1];
          if (isAbove)
            rectangleCount += clockwiseCountRectangles(coord2, coordsTable, RIGHT, lowerLeftY);
        }
      } else if (direction == RIGHT) {
        List<Integer[]> relevantCoords = coordsTable.get("y").get(coord1[1]);
        for (Integer[] coord2 : relevantCoords) {
          boolean isRight = coord2[0] > coord1[0];
          if (isRight)
            rectangleCount += clockwiseCountRectangles(coord2, coordsTable, DOWN, lowerLeftY);
        }
      }
      return rectangleCount;
    }
  }
}

```

### Solution 3

```java
class Program {
  // O(n^2) time | O(n) space - where n is the number of coordinates
  public static int rectangleMania(List<Integer[]> coords) {
    Set<String> coordsTable = getCoordsTable(coords);
    return getRectangleCount(coords, coordsTable);
  }

  public static Set<String> getCoordsTable(List<Integer[]> coords) {
    Set<String> coordsTable = new HashSet<String>();
    for (Integer[] coord : coords) {
      String coordString = coordToString(coord);
      coordsTable.add(coordString);
    }
    return coordsTable;
  }

  public static int getRectangleCount(List<Integer[]> coords, Set<String> coordsTable) {
    int rectangleCount = 0;
    for (Integer[] coord1 : coords) {
      for (Integer[] coord2 : coords) {
        if (!isInUpperRight(coord1, coord2)) continue;
        String upperCoordString = coordToString(new Integer[]{coord1[0], coord2[1]});
        String rightCoordString = coordToString(new Integer[]{coord2[0], coord1[1]});
        if (coordsTable.contains(upperCoordString) && coordsTable.contains(rightCoordString))
          rectangleCount++;
      }
    }
    return rectangleCount;
  }

  public static boolean isInUpperRight(Integer[] coord1, Integer[] coord2) {
    return coord2[0] > coord1[0] && coord2[1] > coord1[1];
  }

  public static String coordToString(Integer[] coord) {
    return Integer.toString(coord[0]) + "-" + Integer.toString(coord[1]);
  }
}

```
