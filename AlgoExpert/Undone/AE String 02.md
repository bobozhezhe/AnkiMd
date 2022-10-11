# AlgoExpert

## String 02 Caesar Cipher Encryptor

### Question

Given a non-empty string of lowercase letters and a non-negative integer representing a key, write a function that returns a new string obtained by shifting every letter in the input string by k positions in the alphabet, where k is the key.

Note that letters should "wrap" around the alphabet; in other words, the letter z shifted by one returns the letter a.

### Sample Input

string = "xyz"
key = 2

### Sample Output

"zab"

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space
  public static String caesarCypherEncryptor(String str, int key) {
    char[] newLetters = new char[str.length()];
    int newKey = key % 26;
    for (int i = 0; i < str.length(); i++) {
      newLetters[i] = getNewLetter(str.charAt(i), newKey);
    }
    return new String(newLetters);
  }

  public static char getNewLetter(char letter, int key) {
    int newLetterCode = letter + key;
    return newLetterCode <= 122 ? (char) newLetterCode : (char) (96 + newLetterCode % 122);
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space
  public static String caesarCypherEncryptor(String str, int key) {
    char[] newLetters = new char[str.length()];
    int newKey = key % 26;
    String alphabet = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < str.length(); i++) {
      newLetters[i] = getNewLetter(str.charAt(i), newKey, alphabet);
    }
    return new String(newLetters);
  }

  public static char getNewLetter(char letter, int key, String alphabet) {
    int newLetterCode = alphabet.indexOf(letter) + key;
    return alphabet.charAt(newLetterCode % 26);
  }
}

```
