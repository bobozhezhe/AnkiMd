# AlgoExpert

## Dynamic Programming 16 Palindrome Partitioning Min Cuts

### Question

Given a non-empty string, write a function that returns the minimum number of cuts needed to perform on the string such that each remaining substring is a palindrome.

A palindrome is defined as a string that's written the same forward as backward. Note that single-character strings are palindromes.

### Sample Input

string = "noonabbad"

### Sample Output

2 // noon | abba | d"

### Optimal Space & Time Complexity

O(n^2) time | O(n^2) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^3) time | O(n^2) space
  public static int palindromePartitioningMinCuts(String str) {
    boolean[][] palindromes = new boolean[str.length()][str.length()];
    for (int i = 0; i < str.length(); i++) {
      for (int j = i; j < str.length(); j++) {
        palindromes[i][j] = isPalindrome(str.substring(i, j + 1));
      }
    }
    int[] cuts = new int[str.length()];
    Arrays.fill(cuts, Integer.MAX_VALUE);
    for (int i = 0; i < str.length(); i++) {
      if (palindromes[0][i]) {
        cuts[i] = 0;
      } else {
        cuts[i] = cuts[i - 1] + 1;
        for (int j = 1; j < i; j++) {
          if (palindromes[j][i] && cuts[j - 1] + 1 < cuts[i]) {
            cuts[i] = cuts[j - 1] + 1;
          }
        }
      }
    }
    return cuts[str.length() - 1];
  }

  public static boolean isPalindrome(String str) {
    int leftIdx = 0;
    int rightIdx = str.length() - 1;
    while (leftIdx < rightIdx) {
      if (str.charAt(leftIdx) != str.charAt(rightIdx)) {
        return false;
      }
      leftIdx++;
      rightIdx--;
    }
    return true;
  }
}

```

### Solution 2

```java
class Program {
  // O(n^2) time | O(n^2) space
  public static int palindromePartitioningMinCuts(String str) {
    boolean[][] palindromes = new boolean[str.length()][str.length()];
    for (int i = 0; i < str.length(); i++) {
      for (int j = 0; j < str.length(); j++) {
        if (i == j) {
          palindromes[i][j] = true;
        } else {
          palindromes[i][j] = false;
        }
      }
    }
    for (int length = 2; length < str.length() + 1; length++) {
      for (int i = 0; i < str.length() - length + 1; i++) {
        int j = i + length - 1;
        if (length == 2) {
          palindromes[i][j] = (str.charAt(i) == str.charAt(j));
        } else {
          palindromes[i][j] = (str.charAt(i) == str.charAt(j) && palindromes[i + 1][j - 1]);
        }
      }
    }
    int[] cuts = new int[str.length()];
    Arrays.fill(cuts, Integer.MAX_VALUE);
    for (int i = 0; i < str.length(); i++) {
      if (palindromes[0][i]) {
        cuts[i] = 0;
      } else {
        cuts[i] = cuts[i - 1] + 1;
        for (int j = 1; j < i; j++) {
          if (palindromes[j][i] && cuts[j - 1] + 1 < cuts[i]) {
            cuts[i] = cuts[j - 1] + 1;
          }
        }
      }
    }
    return cuts[str.length() - 1];
  }
}

```
