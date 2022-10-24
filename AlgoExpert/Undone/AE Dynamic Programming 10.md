# AlgoExpert

## Dynamic Programming 10 Knapsack Problem

### Question

You're given an array of arrays where each subarray holds two integer values and represents an item; the first integer is the item's value, and the second integer is the item's weight. You're also given an integer representing the maximum capacity of a knapsack that you have.

Your goal is to fit items in your knapsack without having the sum of their weights exceed the knapsack's capacity, all the while maximizing their combined value. Note that you only have one of each item at your disposal.

Write a function that returns the maximized combined value of the items that you should pick as well as an array of the indices of each item picked.

If there are multiple combinations of items that maximize the total value in the knapsack, your function can return any of them.

### Sample Input

items = [[1, 2], [4, 3], [5, 6], [6, 7]]
capacity = 10

### Sample Output

[10, [1, 3]] // items [4, 3] and [6, 7]

### Optimal Space & Time Complexity

O(nc) time | O(nc) space - where n is the number of items and c is the capacity

%

### Key Point

### Solution 1

```java
class Program {
  // O(nc) time | O(nc) space
  public static List<List<Integer>> knapsackProblem(int[][] items, int capacity) {
    int[][] knapsackValues = new int[items.length + 1][capacity + 1];
    for (int i = 1; i < items.length + 1; i++) {
      int currentWeight = items[i - 1][1];
      int currentValue = items[i - 1][0];
      for (int c = 0; c < capacity + 1; c++) {
        if (currentWeight > c) {
          knapsackValues[i][c] = knapsackValues[i - 1][c];
        } else {
          knapsackValues[i][c] =
              Math.max(
                  knapsackValues[i - 1][c],
                  knapsackValues[i - 1][c - currentWeight] + currentValue);
        }
      }
    }
    return getKnapsackItems(knapsackValues, items, knapsackValues[items.length][capacity]);
  }

  public static List<List<Integer>> getKnapsackItems(
      int[][] knapsackValues, int[][] items, int weight) {
    List<List<Integer>> sequence = new ArrayList<List<Integer>>();
    List<Integer> totalWeight = new ArrayList<Integer>();
    totalWeight.add(weight);
    sequence.add(totalWeight);
    sequence.add(new ArrayList<Integer>());
    int i = knapsackValues.length - 1;
    int c = knapsackValues[0].length - 1;
    while (i > 0) {
      if (knapsackValues[i][c] == knapsackValues[i - 1][c]) {
        i--;
      } else {
        sequence.get(1).add(0, i - 1);
        c -= items[i - 1][1];
        i--;
      }
      if (c == 0) {
        break;
      }
    }
    return sequence;
  }
}

```
