# AlgoExpert

## Stack 01 Min Max Stack Construction

### Question

Write a MinMaxStack class for a Min Max Stack. The class should support:

Pushing and popping values on and off the stack.
Peeking at the value at the top of the stack.
Getting both the minimum and the maximum values in the stack at any given point in time.
All class methods, when considered independently, should run in constant time and with constant space.

### Sample Usage

// All operations below are performed sequentially.
MinMaxStack(): - // instantiate a MinMaxStack
push(5): -
getMin(): 5
getMax(): 5
peek(): 5
push(7): -
getMin(): 5
getMax(): 7
peek(): 7
push(2): -
getMin(): 2
getMax(): 7
peek(): 2
pop(): 2
pop(): 7
getMin(): 5
getMax(): 5
peek(): 5

### Optimal Space & Time Complexity

All methods: O(1) time | O(1) space

%

### Key Point

### Solution 1

```java
class Program {
  static class MinMaxStack {
    List<Map<String, Integer>> minMaxStack = new ArrayList<Map<String, Integer>>();
    List<Integer> stack = new ArrayList<Integer>();

    // O(1) time | O(1) space
    public int peek() {
      return stack.get(stack.size() - 1);
    }

    // O(1) time | O(1) space
    public int pop() {
      minMaxStack.remove(minMaxStack.size() - 1);
      return stack.remove(stack.size() - 1);
    }

    // O(1) time | O(1) space
    public void push(int number) {
      Map<String, Integer> newMinMax = new HashMap<String, Integer>();
      newMinMax.put("min", number);
      newMinMax.put("max", number);
      if (minMaxStack.size() > 0) {
        Map<String, Integer> lastMinMax =
            new HashMap<String, Integer>(minMaxStack.get(minMaxStack.size() - 1));
        newMinMax.replace("min", Math.min(lastMinMax.get("min"), number));
        newMinMax.replace("max", Math.max(lastMinMax.get("max"), number));
      }
      minMaxStack.add(newMinMax);
      stack.add(number);
    }

    // O(1) time | O(1) space
    public int getMin() {
      return minMaxStack.get(minMaxStack.size() - 1).get("min");
    }

    // O(1) time | O(1) space
    public int getMax() {
      return minMaxStack.get(minMaxStack.size() - 1).get("max");
    }
  }
}

```
