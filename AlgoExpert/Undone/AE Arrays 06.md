# AlgoExpert

## Arrays 06 Three Number Sum

### Question

Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

If no three numbers sum up to the target sum, the function should return an empty array.

### Sample Input

array = [12, 3, 1, 2, -6, 5, -8, 6]
targetSum = 0

### Sample Output

[[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]

### Optimal Space & Time Complexity

O(n^2) time | O(n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n^2) time | O(n) space
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum) {
    Arrays.sort(array);
    List<Integer[]> triplets = new ArrayList<Integer[]>();
    for (int i = 0; i < array.length - 2; i++) {
      int left = i + 1;
      int right = array.length - 1;
      while (left < right) {
        int currentSum = array[i] + array[left] + array[right];
        if (currentSum == targetSum) {
          Integer[] newTriplet = {array[i], array[left], array[right]};
          triplets.add(newTriplet);
          left++;
          right--;
        } else if (currentSum < targetSum) {
          left++;
        } else if (currentSum > targetSum) {
          right--;
        }
      }
    }
    return triplets;
  }
}

```
