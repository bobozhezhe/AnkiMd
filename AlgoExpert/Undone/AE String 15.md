# AlgoExpert

## String 15 Longest Balanced Substring

### Question

Write a function that takes in a string made up of parentheses (( and )). The function should return an integer representing the length of the longest balanced substring with regards to parentheses.

A string is said to be balanced if it has as many opening parentheses as it has closing parentheses and if no parenthesis is unmatched. Note that an opening parenthesis can't match a closing parenthesis that comes before it, and similarly, a closing parenthesis can't match an opening parenthesis that comes after it.

### Sample Input

string = "(()))("

### Sample Output

4 // The longest balanced substring is "(())".

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^3) time | O(n) space - where n is the length of the input string
  public int longestBalancedSubstring(String string) {
    int maxLength = 0;

    for (int i = 0; i < string.length(); i++) {
      for (int j = i + 2; j < string.length() + 1; j += 2) {
        if (isBalanced(string.substring(i, j))) {
          int currentLength = j - i;
          maxLength = Math.max(maxLength, currentLength);
        }
      }
    }

    return maxLength;
  }

  public boolean isBalanced(String string) {
    Stack<Character> openParensStack = new Stack();

    for (int i = 0; i < string.length(); i++) {
      char c = string.charAt(i);
      if (c == '(') {
        openParensStack.push('(');
      } else if (openParensStack.size() > 0) {
        openParensStack.pop();
      } else {
        return false;
      }
    }

    return openParensStack.size() == 0;
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the input string
  public int longestBalancedSubstring(String string) {
    int maxLength = 0;
    Stack<Integer> idxStack = new Stack();
    idxStack.push(-1);

    for (int i = 0; i < string.length(); i++) {
      if (string.charAt(i) == '(') {
        idxStack.push(i);
      } else {
        idxStack.pop();
        if (idxStack.size() == 0) {
          idxStack.push(i);
        } else {
          int balancedSubstringStartIdx = idxStack.peek();
          int currentLength = i - balancedSubstringStartIdx;
          maxLength = Math.max(maxLength, currentLength);
        }
      }
    }

    return maxLength;
  }
}

```

### Solution 3

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the input string
  public int longestBalancedSubstring(String string) {
    int maxLength = 0;

    int openingCount = 0;
    int closingCount = 0;

    for (int i = 0; i < string.length(); i++) {
      char c = string.charAt(i);

      if (c == '(') {
        openingCount += 1;
      } else {
        closingCount += 1;
      }

      if (openingCount == closingCount) {
        maxLength = Math.max(maxLength, closingCount * 2);
      } else if (closingCount > openingCount) {
        openingCount = 0;
        closingCount = 0;
      }
    }

    openingCount = 0;
    closingCount = 0;

    for (int i = string.length() - 1; i >= 0; i--) {
      char c = string.charAt(i);

      if (c == '(') {
        openingCount += 1;
      } else {
        closingCount += 1;
      }

      if (openingCount == closingCount) {
        maxLength = Math.max(maxLength, openingCount * 2);
      } else if (openingCount > closingCount) {
        openingCount = 0;
        closingCount = 0;
      }
    }

    return maxLength;
  }
}

```

### Solution 4

```java
class Program {
  // O(n) time | O(1) space - where n is the length of the input string
  public int longestBalancedSubstring(String string) {
    return Math.max(
        getLongestBalancedInDirection(string, true), getLongestBalancedInDirection(string, false));
  }

  public int getLongestBalancedInDirection(String string, Boolean leftToRight) {
    char openingParens = leftToRight ? '(' : ')';
    int startIdx = leftToRight ? 0 : string.length() - 1;
    int step = leftToRight ? 1 : -1;

    int maxLength = 0;

    int openingCount = 0;
    int closingCount = 0;

    int idx = startIdx;
    while (idx >= 0 && idx < string.length()) {
      char c = string.charAt(idx);

      if (c == openingParens) {
        openingCount += 1;
      } else {
        closingCount += 1;
      }

      if (openingCount == closingCount) {
        maxLength = Math.max(maxLength, closingCount * 2);
      } else if (closingCount > openingCount) {
        openingCount = 0;
        closingCount = 0;
      }

      idx += step;
    }

    return maxLength;
  }
}

```
