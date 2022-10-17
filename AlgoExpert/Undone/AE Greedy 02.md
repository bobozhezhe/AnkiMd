# AlgoExpert

## Greedy 02 Class Photos

### Question

It's photo day at the local school, and you're the photographer assigned to take class photos. The class that you'll be photographing has an even number of students, and all these students are wearing red or blue shirts. In fact, exactly half of the class is wearing red shirts, and the other half is wearing blue shirts. You're responsible for arranging the students in two rows before taking the photo. Each row should contain the same number of the students and should adhere to the following guidelines:

All students wearing red shirts must be in the same row.
All students wearing blue shirts must be in the same row.
Each student in the back row must be strictly taller than the student directly in front of them in the front row.
You're given two input arrays: one containing the heights of all the students with red shirts and another one containing the heights of all the students with blue shirts. These arrays will always have the same length, and each height will be a positive integer. Write a function that returns whether or not a class photo that follows the stated guidelines can be taken.

Note: you can assume that each class has at least 2 students.

### Sample Input

redShirtHeights = [5, 8, 1, 3, 4]
blueShirtHeights = [6, 9, 2, 4, 5]

### Sample Output

true // Place all students with blue shirts in the back row.

### Optimal Space & Time Complexity

O(nlog(n)) time | O(1) space - where n is the number of students

%

### Key Point

### Solution 1

```java
class Program {

  // O(nlog(n)) time | O(1) space - where n is the number of students
  public boolean classPhotos(
      ArrayList<Integer> redShirtHeights, ArrayList<Integer> blueShirtHeights) {
    Collections.sort(redShirtHeights, Collections.reverseOrder());
    Collections.sort(blueShirtHeights, Collections.reverseOrder());

    String shirtColorInFirstRow =
        (redShirtHeights.get(0) < blueShirtHeights.get(0)) ? "RED" : "BLUE";
    for (int idx = 0; idx < redShirtHeights.size(); idx++) {
      int redShirtHeight = redShirtHeights.get(idx);
      int blueShirtHeight = blueShirtHeights.get(idx);

      if (shirtColorInFirstRow == "RED") {
        if (redShirtHeight >= blueShirtHeight) {
          return false;
        }
      } else {
        if (blueShirtHeight >= redShirtHeight) {
          return false;
        }
      }
    }

    return true;
  }
}

```
