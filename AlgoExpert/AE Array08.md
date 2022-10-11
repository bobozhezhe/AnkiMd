# AlgoExpert

## Array 08 Move Element To End

### Question

You're given an array of integers and an integer. Write a function that moves all instances of that integer in the array to the end of the array and returns the array.

The function should perform this in place (i.e., it should mutate the input array) and doesn't need to maintain the order of the other integers.

### Sample Input

array = [2, 1, 2, 2, 2, 3, 4, 2]
toMove = 2

### Sample Output

[1, 3, 4, 2, 2, 2, 2, 2] // the numbers 1, 3, and 4 could be ordered differently

%

### Key Point

### Solution 1

```java
class Program {
  public static List<Integer> moveElementToEnd(List<Integer> array, int toMove) {
    // Write your code here.
    int len = array.size();
    if(len == 0) return array;

    int left = 0, right = len - 1;

    while(left < right) {
        while(left < right && array.get(right) == toMove) {
            right--;
        }

        if(array.get(left).equals(toMove)) {
            int tmp = array.get(left);
            array.set(left, array.get(right));
            array.set(right, tmp);
        }
        left++;
    }

    return array;
  }
}
```
