# AlgoExpert

## String 05 First Non-Repeating Character

### Question

Write a function that takes in a string of lowercase English-alphabet letters and returns the index of the string's first non-repeating character.

The first non-repeating character is the first character in a string that occurs only once.

If the input string doesn't have any non-repeating characters, your function should return -1.

### Sample Input

string = "abcdcaf"

### Sample Output

1 // The first non-repeating character is "b" and is found at index 1.

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input string The constant space is because the input string only has lowercase English-alphabet letters; thus, our hash table will never have more than 26 character frequencies.

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(1) space - where n is the length of the input string
  public int firstNonRepeatingCharacter(String string) {
    for (int idx = 0; idx < string.length(); idx++) {
      boolean foundDuplicate = false;
      for (int idx2 = 0; idx2 < string.length(); idx2++) {
        if (string.charAt(idx) == string.charAt(idx2) && idx != idx2) {
          foundDuplicate = true;
        }
      }

      if (!foundDuplicate) return idx;
    }

    return -1;
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(1) space - where n is the length of the input string
// The constant space is because the input string only has lowercase
// English-alphabet letters; thus, our hash table will never have more
// than 26 character frequencies.
  public int firstNonRepeatingCharacter(String string) {
    HashMap<Character, Integer> characterFrequencies = new HashMap<Character, Integer>();

    for (int idx = 0; idx < string.length(); idx++) {
      char character = string.charAt(idx);
      characterFrequencies.put(character, characterFrequencies.getOrDefault(character, 0) + 1);
    }

    for (int idx = 0; idx < string.length(); idx++) {
      char character = string.charAt(idx);
      if (characterFrequencies.get(character) == 1) {
        return idx;
      }
    }

    return -1;
  }
}

```
