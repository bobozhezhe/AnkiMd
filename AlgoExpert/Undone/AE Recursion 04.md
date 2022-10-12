# AlgoExpert

## Recursion 04 Powerset

### Question

Write a function that takes in an array of unique integers and returns its powerset.

The powerset P(X) of a set X is the set of all subsets of X. For example, the powerset of [1,2] is [[], [1], [2], [1,2]].

Note that the sets in the powerset do not need to be in any particular order.

### Sample Input

array = [1, 2, 3]

### Sample Output

[[], [1], [2], [3], [1, 2], [1, 3], [2, 3], [1, 2, 3]]

### Optimal Space & Time Complexity

O(n*2^n) time | O(n*2^n) space - where n is the length of the input array

%

### Key Point

### Solution 1

```java
class Program {
  // O(n*2^n) time | O(n*2^n) space
  public static List<List<Integer>> powerset(List<Integer> array) {
    return powerset(array, array.size() - 1);
  }

  public static List<List<Integer>> powerset(List<Integer> array, int idx) {
    if (idx < 0) {
      List<List<Integer>> emptySet = new ArrayList<List<Integer>>();
      emptySet.add(new ArrayList<Integer>());
      return emptySet;
    }
    int ele = array.get(idx);
    List<List<Integer>> subsets = powerset(array, idx - 1);
    int length = subsets.size();
    for (int i = 0; i < length; i++) {
      List<Integer> currentSubset = new ArrayList<Integer>(subsets.get(i));
      currentSubset.add(ele);
      subsets.add(currentSubset);
    }
    return subsets;
  }
}

```

### Solution 2

```java
class Program {
  // O(n*2^n) time | O(n*2^n) space
  public static List<List<Integer>> powerset(List<Integer> array) {
    List<List<Integer>> subsets = new ArrayList<List<Integer>>();
    subsets.add(new ArrayList<Integer>());
    for (Integer ele : array) {
      int length = subsets.size();
      for (int i = 0; i < length; i++) {
        List<Integer> currentSubset = new ArrayList<Integer>(subsets.get(i));
        currentSubset.add(ele);
        subsets.add(currentSubset);
      }
    }
    return subsets;
  }
}

```
