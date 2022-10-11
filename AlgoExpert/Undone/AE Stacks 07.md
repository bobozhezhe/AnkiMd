# AlgoExpert

## Stack 07 Largest Rectangle Under Skyline

### Question

Write a function that takes in an array of positive integers representing the heights of adjacent buildings and returns the area of the largest rectangle that can be created by any number of adjacent buildings, including just one building. Note that all buildings have the same width of 1 unit.

For example, given buildings = [2, 1, 2], the area of the largest rectangle that can be created is 3, using all three buildings. Since the minimum height of the three buildings is 1, you can create a rectangle that has a height of 1 and a width of 3 (the number of buildings). You could also create rectangles of area 2 by using only the first building or the last building, but these clearly wouldn't be the largest rectangles. Similarly, you could create rectangles of area 2 by using the first and second building or the second and third building.

To clarify, the width of a created rectangle is the number of buildings used to create the rectangle, and its height is the height of the smallest building used to create it.

Note that if no rectangles can be created, your function should return 0.

### Sample Input

buildings = [1, 3, 3, 2, 4, 1, 5, 3, 2]

### Sample Output

9

```c
// Below is a visual representation of the sample input.
//              _
//          _  | |
//    _ _  | | | |_
//   | | |_| | | | |_
//  _| | | | |_| | | |
// |_|_|_|_|_|_|_|_|_|
```

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the number of buildings

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(1) space - where n is the number of buildings
  public int largestRectangleUnderSkyline(ArrayList<Integer> buildings) {
    int maxArea = 0;
    for (int pillarIdx = 0; pillarIdx < buildings.size(); pillarIdx++) {
      int currentHeight = buildings.get(pillarIdx);

      int furthestLeft = pillarIdx;
      while (furthestLeft > 0 && buildings.get(furthestLeft - 1) >= currentHeight) {
        furthestLeft -= 1;
      }

      int furthestRight = pillarIdx;
      while (furthestRight < buildings.size() - 1
          && buildings.get(furthestRight + 1) >= currentHeight) {
        furthestRight += 1;
      }

      int areaWithCurrentBuilding = (furthestRight - furthestLeft + 1) * currentHeight;
      maxArea = Math.max(areaWithCurrentBuilding, maxArea);
    }

    return maxArea;
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(n) space - where n is the number of buildings
  public int largestRectangleUnderSkyline(ArrayList<Integer> buildings) {
    Stack<Integer> pillarIndices = new Stack<Integer>();
    int maxArea = 0;

    ArrayList<Integer> extendedBuildings = new ArrayList<Integer>(buildings);
    extendedBuildings.add(0);
    for (int idx = 0; idx < extendedBuildings.size(); idx++) {
      int height = extendedBuildings.get(idx);
      while (!pillarIndices.isEmpty() && extendedBuildings.get(pillarIndices.peek()) >= height) {
        int pillarHeight = extendedBuildings.get(pillarIndices.pop());
        int width = (pillarIndices.isEmpty()) ? idx : idx - pillarIndices.peek() - 1;
        maxArea = Math.max(width * pillarHeight, maxArea);
      }

      pillarIndices.push(idx);
    }

    return maxArea;
  }
}

```
