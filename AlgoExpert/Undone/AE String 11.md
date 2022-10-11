# AlgoExpert

## String 11 Longest Substring Without Duplication

### Question

Write a function that takes in a string and returns its longest substring without duplicate characters.

You can assume that there will only be one longest substring without duplication.

### Sample Input

string = "clementisacap"

### Sample Output

"mentisac"

### Optimal Space & Time Complexity

O(n) time | O(min(n, a)) space - where n is the length of the input string and a is the length of the character alphabet represented in the input string
%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(min(n, a)) space
  public static String longestSubstringWithoutDuplication(String str) {
    Map<Character, Integer> lastSeen = new HashMap<Character, Integer>();
    int[] longest = {0, 1};
    int startIdx = 0;
    for (int i = 0; i < str.length(); i++) {
      char c = str.charAt(i);
      if (lastSeen.containsKey(c)) {
        startIdx = Math.max(startIdx, lastSeen.get(c) + 1);
      }
      if (longest[1] - longest[0] < i + 1 - startIdx) {
        longest = new int[] {startIdx, i + 1};
      }
      lastSeen.put(c, i);
    }
    String result = str.substring(longest[0], longest[1]);
    return result;
  }
}

```
