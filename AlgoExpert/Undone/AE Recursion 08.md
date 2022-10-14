# AlgoExpert

## Recursion 08 Interweaving Strings

### Question

Write a function that takes in three strings and returns a boolean representing whether the third string can be formed by interweaving the first two strings.

To interweave strings means to merge them by alternating their letters without any specific pattern. For instance, the strings "abc" and "123" can be interwoven as "a1b2c3", as "abc123", and as "ab1c23" (this list is nonexhaustive).

Letters within a string must maintain their relative ordering in the interwoven string.

### Sample Input

one = "algoexpert"
two = "your-dream-job"
three = "your-algodream-expertjob"

### Sample Output

true

### Optimal Space & Time Complexity

O(nm) time | O(nm) space - where n is the length of the first string and m is the length of the second string

%

### Key Point

### Solution 1

```java
class Program {
  // O(2^(n + m)) time | O(n + m) space - where n is the length
  // of the first string and m is the length of the second string
  public static boolean interweavingStrings(String one, String two, String three) {
    if (three.length() != one.length() + two.length()) {
      return false;
    }

    return areInterwoven(one, two, three, 0, 0);
  }

  public static boolean areInterwoven(String one, String two, String three, int i, int j) {
    int k = i + j;
    if (k == three.length()) return true;

    if (i < one.length() && one.charAt(i) == three.charAt(k)) {
      if (areInterwoven(one, two, three, i + 1, j)) return true;
    }

    if (j < two.length() && two.charAt(j) == three.charAt(k)) {
      return areInterwoven(one, two, three, i, j + 1);
    }

    return false;
  }
}

```

### Solution 2

```java
class Program {
  // O(nm) time | O(nm) space - where n is the length of the
  // first string and m is the length of the second string
  public static boolean interweavingStrings(String one, String two, String three) {
    if (three.length() != one.length() + two.length()) {
      return false;
    }

    Boolean[][] cache = new Boolean[one.length() + 1][two.length() + 1];
    return areInterwoven(one, two, three, 0, 0, cache);
  }

  public static boolean areInterwoven(
      String one, String two, String three, int i, int j, Boolean[][] cache) {
    if (cache[i][j] != null) return cache[i][j];

    int k = i + j;
    if (k == three.length()) {
      return true;
    }

    if (i < one.length() && one.charAt(i) == three.charAt(k)) {
      cache[i][j] = areInterwoven(one, two, three, i + 1, j, cache);
      if (cache[i][j]) return true;
    }

    if (j < two.length() && two.charAt(j) == three.charAt(k)) {
      var result = areInterwoven(one, two, three, i, j + 1, cache);
      cache[i][j] = result;
      return result;
    }

    cache[i][j] = false;
    return false;
  }
}

```
