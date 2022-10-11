# AlgoExpert

## String 04 Generate Document

### Question

You're given a string of available characters and a string representing a document that you need to generate. Write a function that determines if you can generate the document using the available characters. If you can generate the document, your function should return true; otherwise, it should return false.

You're only able to generate the document if the frequency of unique characters in the characters string is greater than or equal to the frequency of unique characters in the document string. For example, if you're given characters = "abcabc" and document = "aabbccc" you cannot generate the document because you're missing one c.

The document that you need to create may contain any characters, including special characters, capital letters, numbers, and spaces.

Note: you can always generate the empty string ("").

### Sample Input

characters = "Bste!hetsi ogEAxpelrt x "
document = "AlgoExpert is the Best!"

### Sample Output

true

### Optimal Space & Time Complexity

O(n + m) time | O(c) space - where n is the number of characters, m is the length of the document, and c is the number of unique characters in the characters string

%

### Key Point

### Solution 1

```java
class Program {

  // O(m * (n + m)) time | O(1) space - where n is the number
  // of characters and m is the length of the document
  public boolean generateDocument(String characters, String document) {
    for (int idx = 0; idx < document.length(); idx++) {
      char character = document.charAt(idx);
      int documentFrequency = countCharacterFrequency(character, document);
      int charactersFrequency = countCharacterFrequency(character, characters);
      if (documentFrequency > charactersFrequency) {
        return false;
      }
    }

    return true;
  }

  public int countCharacterFrequency(char character, String target) {
    int frequency = 0;
    for (int idx = 0; idx < target.length(); idx++) {
      char c = target.charAt(idx);
      if (c == character) {
        frequency += 1;
      }
    }

    return frequency;
  }
}

```

### Solution 2

```java
class Program {

  // O(c * (n + m)) time | O(c) space - where n is the number of characters, m is
  // the length of the document, and c is the number of unique characters in the
  // document
  public boolean generateDocument(String characters, String document) {
    Set<Character> alreadyCounted = new HashSet<Character>();

    for (int idx = 0; idx < document.length(); idx++) {
      char character = document.charAt(idx);
      if (alreadyCounted.contains(character)) {
        continue;
      }

      int documentFrequency = countCharacterFrequency(character, document);
      int charactersFrequency = countCharacterFrequency(character, characters);
      if (documentFrequency > charactersFrequency) {
        return false;
      }

      alreadyCounted.add(character);
    }

    return true;
  }

  public int countCharacterFrequency(char character, String target) {
    int frequency = 0;
    for (int idx = 0; idx < target.length(); idx++) {
      char c = target.charAt(idx);
      if (c == character) {
        frequency += 1;
      }
    }

    return frequency;
  }
}

```

### Solution 3

```java
class Program {

  // O(n + m) time | O(c) space - where n is the number of characters, m is
  // the length of the document, and c is the number of unique characters in the
  // characters string
  public boolean generateDocument(String characters, String document) {
    HashMap<Character, Integer> characterCounts = new HashMap<Character, Integer>();

    for (int idx = 0; idx < characters.length(); idx++) {
      char character = characters.charAt(idx);
      characterCounts.put(character, characterCounts.getOrDefault(character, 0) + 1);
    }

    for (int idx = 0; idx < document.length(); idx++) {
      char character = document.charAt(idx);
      if (!characterCounts.containsKey(character) || characterCounts.get(character) == 0) {
        return false;
      }

      characterCounts.put(character, characterCounts.get(character) - 1);
    }

    return true;
  }
}

```
