# AlgoExpert
## Array 15 Four Number Sum
Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all quadruplets in the array that sum up to the target sum and return a two-dimensional array of all these quadruplets in no particular order.

If no four numbers sum up to the target sum, the function should return an empty array.

Sample Input
array = [7, 6, 4, -1, 1, 2]
targetSum = 16
Sample Output
[[7, 6, 4, -1], [7, 6, 1, 2]] // the quadruplets could be ordered differently

%

### Solition
```java
class Program{
	public static List<Integer[]> fourNumberSum(int[] array, int targetSum){
		int len = array.length;
		List<Integer[]> res = new ArrayList<Integer[]>();
		if(len < 4) return res;

		Arrays.sort(array);
		for(int i = 0; i < len - 2; ++i){
			for(int j = i + 1; j < len -1; ++j){
				int diff = targetSum - array[i] - array[j];
				int low = j + 1, high = len -1;
				while(low < high){
					int left = array[low], right = array[high];
					int sum = left + right;
					if(sum < diff){
						low++;
						while(low < high && array[low] == left) low++;
					} else if(sum > diff) {
						high--;
						while(low < high && array[high] == right) high--;
					} else {
						res.add(new Integer[]{array[i], array[j], left, right});
						low++;
						high--;
						while(low < high && array[low] == left) low++;
						while(low < high && array[high] == right) high--;
					}
				}
			}
		}
		return res;
	}
}
```
