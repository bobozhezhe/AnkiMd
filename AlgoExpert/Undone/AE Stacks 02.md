# AlgoExpert

## Stack 02 Balanced Brackets

### Question

Write a function that takes in a string made up of brackets ((, [, {, ), ], and }) and other optional characters. The function should return a boolean representing whether the string is balanced with regards to brackets.

A string is said to be balanced if it has as many opening brackets of a certain type as it has closing brackets of that type and if no bracket is unmatched. Note that an opening bracket can't match a corresponding closing bracket that comes before it, and similarly, a closing bracket can't match a corresponding opening bracket that comes after it. Also, brackets can't overlap each other as in [(]).

### Sample Input

string = "([])(){}(())()()"

### Sample Output

true // it's balanced

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the input string

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space
  public static boolean balancedBrackets(String str) {
    String openingBrackets = "([{";
    String closingBrackets = ")]}";
    Map<Character, Character> matchingBrackets = new HashMap<Character, Character>();
    matchingBrackets.put(')', '(');
    matchingBrackets.put(']', '[');
    matchingBrackets.put('}', '{');
    List<Character> stack = new ArrayList<Character>();
    for (int i = 0; i < str.length(); i++) {
      char letter = str.charAt(i);
      if (openingBrackets.indexOf(letter) != -1) {
        stack.add(letter);
      } else if (closingBrackets.indexOf(letter) != -1) {
        if (stack.size() == 0) {
          return false;
        }
        if (stack.get(stack.size() - 1) == matchingBrackets.get(letter)) {
          stack.remove(stack.size() - 1);
        } else {
          return false;
        }
      }
    }
    return stack.size() == 0;
  }
}

```
