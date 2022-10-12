# AlgoExpert

## Recursion 01 Nth Fibonacci

### Question

The Fibonacci sequence is defined as follows: the first number of the sequence is 0, the second number is 1, and the nth number is the sum of the (n - 1)th and (n - 2)th numbers. Write a function that takes in an integer n and returns the nth Fibonacci number.

Important note: the Fibonacci sequence is often defined with its first two numbers as F0 = 0 and F1 = 1. For the purpose of this question, the first Fibonacci number is F0; therefore, getNthFib(1) is equal to F0, getNthFib(2) is equal to F1, etc..

### Sample Input #1

n = 2

### Sample Output #1

1 // 0, 1

### Sample Input #2

n = 6

### Sample Output #2

5 // 0, 1, 1, 2, 3, 5

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the input number

%

### Key Point

### Solution 1

```java
class Program {
  // O(2^n) time | O(n) space
  public static int getNthFib(int n) {
    if (n == 2) {
      return 1;
    } else if (n == 1) {
      return 0;
    } else {
      return getNthFib(n - 1) + getNthFib(n - 2);
    }
  }
}

```

### Solution 2

```java
class Program {
  // O(n) time | O(n) space
  public static int getNthFib(int n) {
    Map<Integer, Integer> memoize = new HashMap<Integer, Integer>();
    memoize.put(1, 0);
    memoize.put(2, 1);
    return getNthFib(n, memoize);
  }

  public static int getNthFib(int n, Map<Integer, Integer> memoize) {
    if (memoize.containsKey(n)) {
      return memoize.get(n);
    } else {
      memoize.put(n, getNthFib(n - 1, memoize) + getNthFib(n - 2, memoize));
      return memoize.get(n);
    }
  }
}

```

### Solution 3

```java
class Program {
  // O(n) time | O(1) space
  public static int getNthFib(int n) {
    int[] lastTwo = {0, 1};
    int counter = 3;
    while (counter <= n) {
      int nextFib = lastTwo[0] + lastTwo[1];
      lastTwo[0] = lastTwo[1];
      lastTwo[1] = nextFib;
      counter++;
    }
    return n > 1 ? lastTwo[1] : lastTwo[0];
  }
}

```
