# AlgoExpert

## Stack 04 Sort Stack

### Question

Write a function that takes in an array of integers representing a stack, recursively sorts the stack in place (i.e., doesn't create a brand new array), and returns it.

The array must be treated as a stack, with the end of the array as the top of the stack. Therefore, you're only allowed to

Pop elements from the top of the stack by removing elements from the end of the array using the built-in .pop() method in your programming language of choice.
Push elements to the top of the stack by appending elements to the end of the array using the built-in .append() method in your programming language of choice.
Peek at the element on top of the stack by accessing the last element in the array.
You're not allowed to perform any other operations on the input array, including accessing elements (except for the last element), moving elements, etc.. You're also not allowed to use any other data structures, and your solution must be recursive.

### Sample Input

stack = [-5, 2, -2, 4, 3, 1]

### Sample Output

[-5, -2, 1, 2, 3, 4]

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the length of the stack

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(n) space - where n is the length of the stack
  public ArrayList<Integer> sortStack(ArrayList<Integer> stack) {
    if (stack.size() == 0) {
      return stack;
    }

    int top = stack.remove(stack.size() - 1);

    sortStack(stack);

    insertInSortedOrder(stack, top);

    return stack;
  }

  public void insertInSortedOrder(ArrayList<Integer> stack, int value) {
    if (stack.size() == 0 || (stack.get(stack.size() - 1) <= value)) {
      stack.add(value);
      return;
    }

    int top = stack.remove(stack.size() - 1);

    insertInSortedOrder(stack, value);

    stack.add(top);
  }
}

```
