# AlgoExpert

## Recursion 11 Ambiguous Measurements

### Question

This problem deals with measuring cups that are missing important measuring labels. Specifically, a measuring cup only has two measuring lines, a Low (L) line and a High (H) line. This means that these cups can't precisely measure and can only guarantee that the substance poured into them will be between the L and H line. For example, you might have a measuring cup that has a Low line at 400ml and a high line at 435ml. This means that when you use this measuring cup, you can only be sure that what you're measuring is between 400ml and 435ml.

You're given a list of measuring cups containing their low and high lines as well as one low integer and one high integer representing a range for a target measurement. Write a function that returns a boolean representing whether you can use the cups to accurately measure a volume in the specified [low, high] range (the range is inclusive).

Note that:

Each measuring cup will be represented by a pair of positive integers [L, H], where 0 <= L <= H.
You'll always be given at least one measuring cup, and the low and high input parameters will always satisfy the following constraint: 0 <= low <= high.
Once you've measured some liquid, it will immediately be transferred to a larger bowl that will eventually (possibly) contain the target measurement.
You can't pour the contents of one measuring cup into another cup.

### Sample Input

measuringCups = [
  [200, 210],
  [450, 465],
  [800, 850],
]
low = 2100
high = 2300

### Sample Output

true
// We use cup [450, 465] to measure four volumes:
// First measurement: Low = 450, High = 465
// Second measurement: Low = 450 + 450 = 900, High = 465 + 465 = 930
// Third measurement: Low = 900 + 450 = 1350, High = 930 + 465 = 1395
// Fourth measurement: Low = 1350 + 450 = 1800, High = 1395 + 465 = 1860

// Then we use cup [200, 210] to measure two volumes:
// Fifth measurement: Low = 1800 + 200 = 2000, High = 1860 + 210 = 2070
// Sixth measurement: Low = 2000 + 200 = 2200, High = 2070 + 210 = 2280

// We've measured a volume in the range [2200, 2280].
// This is within our target range, so we return `true`.

// Note: there are other ways to measure a volume in the target range.

### Optimal Space & Time Complexity

O(low *high* n) time | O(low * high) space - where n is the number of measuring cups

%

### Key Point

### Solution 1

```java
class Program {

  // O(low * high * n) time | O(low * high) space - where n is the number of measuring cups
  public boolean ambiguousMeasurements(int[][] measuringCups, int low, int high) {
    HashMap<String, Boolean> memoization = new HashMap<String, Boolean>();
    return canMeasureInRange(measuringCups, low, high, memoization);
  }

  public boolean canMeasureInRange(
      int[][] measuringCups, int low, int high, HashMap<String, Boolean> memoization) {
    String memoizeKey = createHashableKey(low, high);
    if (memoization.containsKey(memoizeKey)) {
      return memoization.get(memoizeKey);
    }

    if (low <= 0 && high <= 0) {
      return false;
    }

    boolean canMeasure = false;
    for (int[] cup : measuringCups) {
      int cupLow = cup[0];
      int cupHigh = cup[1];
      if (low <= cupLow && cupHigh <= high) {
        canMeasure = true;
        break;
      }

      int newLow = Math.max(0, low - cupLow);
      int newHigh = Math.max(0, high - cupHigh);
      canMeasure = canMeasureInRange(measuringCups, newLow, newHigh, memoization);
      if (canMeasure) break;
    }

    memoization.put(memoizeKey, canMeasure);
    return canMeasure;
  }

  public String createHashableKey(int low, int high) {
    return String.valueOf(low) + ":" + String.valueOf(high);
  }
}

```
