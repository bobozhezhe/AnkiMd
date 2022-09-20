# AlgoExpert
## Array 06 Three Number Sum
Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

If no three numbers sum up to the target sum, the function should return an empty array.

### Sample Input
array = [12, 3, 1, 2, -6, 5, -8, 6]
targetSum = 0
### Sample Output
[[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]

%

### Solition
```java
class Program {
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum){
    int len = array.length;
    List<Integer[]> res = new ArrayList<Integer[]>();
    if(len < 3) return res;
    Arrays.sort(array);
  
    for(int i = 0; i < len - 1; ++i){
      int diff = targetSum - array[i];
      int low = i + 1, high = len - 1;
      while(low < high){
        int left = array[low];
        int right = array[high];
        int sum = left + right;
  
        if(sum < diff){
            low++;
            while(low < high && left == array[low]) low++;
        } else if (sum > diff){
            high--;
            while(low < high && right == array[high]) high--;
        } else {
            res.add(new Integer[]{array[i], left, right});
            low++;
            high--;
            while(low < high && left == array[low]) low++;
            while(low < high && right == array[high]) high--;
        }
      }
    }
    return res;
  }
}
```
