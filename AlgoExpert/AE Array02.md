# AlgoExpert
## Array 02 Validate Subsequence
A subsequence of an array is a set of numbers that aren't necessarily adjacent in the array but that are in the same order as they appear in the array. For instance, the numbers [1, 3, 4] form a subsequence of the array [1, 2, 3, 4], and so do the numbers [2, 4]. Note that a single number in an array and the array itself are both valid subsequences of the array.

### Sample Input
array = [5, 1, 22, 25, 6, -1, 8, 10]
sequence = [1, 6, -1, 10]
### Sample Output
true

%

### Solution

```java
class Program {
  public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
    // Write your code here.
    int j = 0;
    for(int i = 0; i < array.size() && j < sequence.size(); ++i){
      if(array.get(i) == sequence.get(j)){
        j++;
      }
    }
    return j == sequence.size();
  }
}
```
