# AlgoExpert

## String 06 Longest Palindromic Substring

### Question

Write a function that, given a string, returns its longest palindromic substring.

A palindrome is defined as a string that's written the same forward and backward. Note that single-character strings are palindromes.

You can assume that there will only be one longest palindromic substring.

### Sample Input

string = "abaxyzzyxf"

### Sample Output

"xyzzyx"

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^3) time | O(n) space
  public static String longestPalindromicSubstring(String str) {
    String longest = "";
    for (int i = 0; i < str.length(); i++) {
      for (int j = i; j < str.length(); j++) {
        String substring = str.substring(i, j + 1);
        if (substring.length() > longest.length() && isPalindrome(substring)) {
          longest = substring;
        }
      }
    }
    return longest;
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
  // O(n^2) time | O(n) space
  public static String longestPalindromicSubstring(String str) {
    int[] currentLongest = {0, 1};
    for (int i = 1; i < str.length(); i++) {
      int[] odd = getLongestPalindromeFrom(str, i - 1, i + 1);
      int[] even = getLongestPalindromeFrom(str, i - 1, i);
      int[] longest = odd[1] - odd[0] > even[1] - even[0] ? odd : even;
      currentLongest =
          currentLongest[1] - currentLongest[0] > longest[1] - longest[0]
              ? currentLongest
              : longest;
    }
    return str.substring(currentLongest[0], currentLongest[1]);
  }

  public static int[] getLongestPalindromeFrom(String str, int leftIdx, int rightIdx) {
    while (leftIdx >= 0 && rightIdx < str.length()) {
      if (str.charAt(leftIdx) != str.charAt(rightIdx)) {
        break;
      }
      leftIdx--;
      rightIdx++;
    }
    return new int[] {leftIdx + 1, rightIdx};
  }
}

```
