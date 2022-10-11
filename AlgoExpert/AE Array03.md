# AlgoExpert

## Array 03 Sorted Squared Array

### Question

Write a function that takes in a non-empty array of integers that are sorted in ascending order and returns a new array of the same length with the squares of the original integers also sorted in ascending order.

### Sample Input

array = [1, 2, 3, 5, 6, 8, 9]

### Sample Output

[1, 4, 9, 25, 36, 64, 81]

%

### Solution

```java
class Program {

  public int[] sortedSquaredArray(int[] array) {
    // Write your code here.
    int n = array.length;
    if(n == 0) return array;
    int[] res = new int[n];

    int left = 0, right = n - 1;
    for(int i = n - 1; i >= 0; --i){
     int lv = array[left], rv = array[right];
      if(Math.abs(rv) >= Math.abs(lv)){
            res[i] = rv * rv;
         right--;
     } else {
            res[i] = lv * lv;
         left++;
     }
    }
    return res;
  }
}

```
