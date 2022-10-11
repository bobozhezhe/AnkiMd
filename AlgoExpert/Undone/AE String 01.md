# AlgoExpert

## String 01 Palindrome Check

### Question

Write a function that takes in a non-empty string and that returns a boolean representing whether the string is a palindrome.

A palindrome is defined as a string that's written the same forward and backward. Note that single-character strings are palindromes.

### Sample Input

string = "abcdcba"

### Sample Output

true // it's written the same forward and backward

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static boolean isPalindrome(String str) {
    String reversedString = "";
    for (int i = str.length() - 1; i >= 0; i--) {
      reversedString += str.charAt(i);
    }
    return str.equals(reversedString);
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space
  public static boolean isPalindrome(String str) {
    StringBuilder reversedString = new StringBuilder();
    for (int i = str.length() - 1; i >= 0; i--) {
      reversedString.append(str.charAt(i));
    }
    return str.equals(reversedString.toString());
  }
}

```

### Solution 3

```java
class Program {
  // O(n) time | O(n) space
  public static boolean isPalindrome(String str) {
    return isPalindrome(str, 0);
  }

  public static boolean isPalindrome(String str, int i) {
    int j = str.length() - 1 - i;
    return i >= j ? true : str.charAt(i) == str.charAt(j) && isPalindrome(str, i + 1);
  }
}

```

### Solution 4

```java
class Program {
  // O(n) time | O(1) space
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
