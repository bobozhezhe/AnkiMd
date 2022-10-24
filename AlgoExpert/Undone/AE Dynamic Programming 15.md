# AlgoExpert

## Dynamic Programming 15 Max Profit With K Transactions

### Question

You're given an array of positive integers representing the prices of a single stock on various days (each index in the array represents a different day). You're also given an integer k, which represents the number of transactions you're allowed to make. One transaction consists of buying the stock on a given day and selling it on another, later day.

Write a function that returns the maximum profit that you can make by buying and selling the stock, given k transactions.

Note that you can only hold one share of the stock at a time; in other words, you can't buy more than one share of the stock on any given day, and you can't buy a share of the stock if you're still holding another share. Also, you don't need to use all k transactions that you're allowed.

### Sample Input

prices = [5, 11, 3, 50, 60, 90]
k = 2

### Sample Output

93 // Buy: 5, Sell: 11; Buy: 3, Sell: 90

### Optimal Space & Time Complexity

O(nk) time | O(n) space - where n is the number of prices and k is the number of transactions

%

### Key Point

### Solution 1

```java
class Program {
  // O(nk) time | O(nk) space
  public static int maxProfitWithKTransactions(int[] prices, int k) {
    if (prices.length == 0) {
      return 0;
    }
    int[][] profits = new int[k + 1][prices.length];
    for (int t = 1; t < k + 1; t++) {
      int maxThusFar = Integer.MIN_VALUE;
      for (int d = 1; d < prices.length; d++) {
        maxThusFar = Math.max(maxThusFar, profits[t - 1][d - 1] - prices[d - 1]);
        profits[t][d] = Math.max(profits[t][d - 1], maxThusFar + prices[d]);
      }
    }
    return profits[k][prices.length - 1];
  }
}

```

### Solution 2

```java
class Program {
  // O(nk) time | O(n) space
  public static int maxProfitWithKTransactions(int[] prices, int k) {
    if (prices.length == 0) {
      return 0;
    }
    int[] evenProfits = new int[prices.length];
    int[] oddProfits = new int[prices.length];

    int[] currentProfits;
    int[] previousProfits;
    for (int t = 1; t < k + 1; t++) {
      int maxThusFar = Integer.MIN_VALUE;
      if (t % 2 == 1) {
        currentProfits = oddProfits;
        previousProfits = evenProfits;
      } else {
        currentProfits = evenProfits;
        previousProfits = oddProfits;
      }
      for (int d = 1; d < prices.length; d++) {
        maxThusFar = Math.max(maxThusFar, previousProfits[d - 1] - prices[d - 1]);
        currentProfits[d] = Math.max(currentProfits[d - 1], maxThusFar + prices[d]);
      }
    }
    return k % 2 == 0 ? evenProfits[prices.length - 1] : oddProfits[prices.length - 1];
  }
}

```
