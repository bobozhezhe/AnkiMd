# AlgoExpert

## Dynamic Programming 04 Levenshtein Distance

### Question

Write a function that takes in two strings and returns the minimum number of edit operations that need to be performed on the first string to obtain the second string.

There are three edit operations: insertion of a character, deletion of a character, and substitution of a character for another.

### Sample Input

str1 = "abc"
str2 = "yabd"

### Sample Output

2 // insert "y"; substitute "c" for "d"

### Optimal Space & Time Complexity

O(nm) time | O(min(n, m)) space - where n and m are the lengths of the two input strings

%

### Key Point

### Solution 1

```java
class Program {
  // O(nm) time | O(nm) space
  public static int levenshteinDistance(String str1, String str2) {
    int[][] edits = new int[str2.length() + 1][str1.length() + 1];
    for (int i = 0; i < str2.length() + 1; i++) {
      for (int j = 0; j < str1.length() + 1; j++) {
        edits[i][j] = j;
      }
      edits[i][0] = i;
    }
    for (int i = 1; i < str2.length() + 1; i++) {
      for (int j = 1; j < str1.length() + 1; j++) {
        if (str2.charAt(i - 1) == str1.charAt(j - 1)) {
          edits[i][j] = edits[i - 1][j - 1];
        } else {
          edits[i][j] =
              1 + Math.min(edits[i - 1][j - 1], Math.min(edits[i - 1][j], edits[i][j - 1]));
        }
      }
    }
    return edits[str2.length()][str1.length()];
  }
}

```

### Solution 2

```java
class Program {
  // O(nm) time | O(min(n, m)) space
  public static int levenshteinDistance(String str1, String str2) {
    String small = str1.length() < str2.length() ? str1 : str2;
    String big = str1.length() >= str2.length() ? str1 : str2;
    int[] evenEdits = new int[small.length() + 1];
    int[] oddEdits = new int[small.length() + 1];
    for (int j = 0; j < small.length() + 1; j++) {
      evenEdits[j] = j;
    }

    int[] currentEdits;
    int[] previousEdits;
    for (int i = 1; i < big.length() + 1; i++) {
      if (i % 2 == 1) {
        currentEdits = oddEdits;
        previousEdits = evenEdits;
      } else {
        currentEdits = evenEdits;
        previousEdits = oddEdits;
      }
      currentEdits[0] = i;
      for (int j = 1; j < small.length() + 1; j++) {
        if (big.charAt(i - 1) == small.charAt(j - 1)) {
          currentEdits[j] = previousEdits[j - 1];
        } else {
          currentEdits[j] =
              1 + Math.min(previousEdits[j - 1], Math.min(previousEdits[j], currentEdits[j - 1]));
        }
      }
    }
    return big.length() % 2 == 0 ? evenEdits[small.length()] : oddEdits[small.length()];
  }
}

```
