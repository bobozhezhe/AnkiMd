# AlgoExpert

## String 09 Reverse Words In String

### Question

Write a function that takes in a string of words separated by one or more whitespaces and returns a string that has these words in reverse order. For example, given the string "tim is great", your function should return "great is tim".

For this problem, a word can contain special characters, punctuation, and numbers. The words in the string will be separated by one or more whitespaces, and the reversed string must contain the same whitespaces as the original string. For example, given the string "whitespaces    4" you would be expected to return "4    whitespaces".

Note that you're not allowed to to use any built-in split or reverse methods/functions. However, you are allowed to use a built-in join method/function.

Also note that the input string isn't guaranteed to always contain words.

### Sample Input

string = "AlgoExpert is the best!"

### Sample Output

"best! the is AlgoExpert"

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the string

%

### Key Point

### Solution 1

```java
class Program {

  // O(n) time | O(n) space - where n is the length of the string
  public String reverseWordsInString(String string) {
    ArrayList<String> words = new ArrayList<String>();
    int startOfWord = 0;

    for (int idx = 0; idx < string.length(); idx++) {
      char character = string.charAt(idx);

      if (character == ' ') {
        words.add(string.substring(startOfWord, idx));
        startOfWord = idx;
      } else if (string.charAt(startOfWord) == ' ') {
        words.add(" ");
        startOfWord = idx;
      }
    }

    words.add(string.substring(startOfWord));

    Collections.reverse(words);
    return String.join("", words);
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(n) space - where n is the length of the string
  public String reverseWordsInString(String string) {
    char[] characters = string.toCharArray();
    reverseListRange(characters, 0, characters.length - 1);

    int startOfWord = 0;
    while (startOfWord < characters.length) {
      int endOfWord = startOfWord;
      while (endOfWord < characters.length && characters[endOfWord] != ' ') {
        endOfWord += 1;
      }

      reverseListRange(characters, startOfWord, endOfWord - 1);
      startOfWord = endOfWord + 1;
    }

    return new String(characters);
  }

  public char[] reverseListRange(char[] list, int start, int end) {
    while (start < end) {
      char temp = list[start];
      list[start] = list[end];
      list[end] = temp;
      start += 1;
      end -= 1;
    }

    return list;
  }
}

```
