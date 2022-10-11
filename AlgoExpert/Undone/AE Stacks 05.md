# AlgoExpert

## Stack 05 Next Greater Element

### Question

Write a function that takes in an array of integers and returns a new array containing, at each index, the next element in the input array that's greater than the element at that index in the input array.

In other words, your function should return a new array where outputArray[i] is the next element in the input array that's greater than inputArray[i]. If there's no such next greater element for a particular index, the value at that index in the output array should be -1. For example, given array = [1, 2], your function should return [2, -1].

Additionally, your function should treat the input array as a circular array. A circular array wraps around itself as if it were connected end-to-end. So the next index after the last index in a circular array is the first index. This means that, for our problem, given array = [0, 0, 5, 0, 0, 3, 0, 0], the next greater element after 3 is 5, since the array is circular.

### Sample Input

array = [2, 5, -3, -4, 6, 7, 2]

### Sample Output

[5, 6, 6, 6, 7, -1, 5]

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the array
  public int[] nextGreaterElement(int[] array) {
    int[] result = new int[array.length];
    Arrays.fill(result, -1);

    Stack<Integer> stack = new Stack<Integer>();

    for (int idx = 0; idx < 2 * array.length; idx++) {
      int circularIdx = idx % array.length;

      while (stack.size() > 0 && array[stack.peek()] < array[circularIdx]) {
        int top = stack.pop();
        result[top] = array[circularIdx];
      }

      stack.push(circularIdx);
    }

    return result;
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the array
  public int[] nextGreaterElement(int[] array) {
    int[] result = new int[array.length];
    Arrays.fill(result, -1);

    Stack<Integer> stack = new Stack<Integer>();

    for (int idx = 2 * array.length - 1; idx >= 0; idx--) {
      int circularIdx = idx % array.length;

      while (stack.size() > 0) {
        if (stack.peek() <= array[circularIdx]) {
          stack.pop();
        } else {
          result[circularIdx] = stack.peek();
          break;
        }
      }

      stack.push(array[circularIdx]);
    }

    return result;
  }
}

```
