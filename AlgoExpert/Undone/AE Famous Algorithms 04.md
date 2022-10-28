# AlgoExpert

## Famous Algorithms 04 Knuth—Morris—Pratt Algorithm

### Question

Write a function that takes in two strings and checks if the first string contains the second one using the Knuth—Morris—Pratt algorithm. The function should return a boolean.

If you're unfamiliar with the Knuth—Morris—Pratt Algorithm, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

string = "aefoaefcdaefcdaed"
substring = "aefcdaed"

### Sample Output

true

### Optimal Space & Time Complexity

O(n + m) time | O(m) space - where n is the length of the main string and m is the length of the potential substring

%

### Key Point

### Solution 1

```java
class Program {
  // O(n + m) time | O(m) space
  public static boolean knuthMorrisPrattAlgorithm(String string, String substring) {
    int[] pattern = buildPattern(substring);
    return doesMatch(string, substring, pattern);
  }

  public static int[] buildPattern(String substring) {
    int[] pattern = new int[substring.length()];
    Arrays.fill(pattern, -1);
    int j = 0;
    int i = 1;
    while (i < substring.length()) {
      if (substring.charAt(i) == substring.charAt(j)) {
        pattern[i] = j;
        i++;
        j++;
      } else if (j > 0) {
        j = pattern[j - 1] + 1;
      } else {
        i++;
      }
    }
    return pattern;
  }

  public static boolean doesMatch(String string, String substring, int[] pattern) {
    int i = 0;
    int j = 0;
    while (i + substring.length() - j <= string.length()) {
      if (string.charAt(i) == substring.charAt(j)) {
        if (j == substring.length() - 1) return true;
        i++;
        j++;
      } else if (j > 0) {
        j = pattern[j - 1] + 1;
      } else {
        i++;
      }
    }
    return false;
  }
}
```
