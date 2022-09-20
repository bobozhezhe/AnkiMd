# AlgoExpert
## Array 07 Smallest Difference
Write a function that takes in two non-empty arrays of integers, finds the pair of numbers (one from each array) whose absolute difference is closest to zero, and returns an array containing these two numbers, with the number from the first array in the first position.

Note that the absolute difference of two integers is the distance between them on the real number line. For example, the absolute difference of -5 and 5 is 10, and the absolute difference of -5 and -4 is 1.

You can assume that there will only be one pair of numbers with the smallest difference.

Sample Input
arrayOne = [-1, 5, 10, 20, 28, 3]
arrayTwo = [26, 134, 135, 15, 17]
Sample Output
[28, 26]

%

### Solition
```java
class Program {
    public static int[] smallestDifference(int[] arrayOne, int[] arrayTwo){
        int len1 = arrayOne.length;
        int len2 = arrayTwo.length;

        Arrays.sort(arrayOne);
        Arrays.sort(arrayTwo);
        int idxOne = 0, idxTwo = 0;
        int minDiff = Integer.MAX_VALUE;
        int minOne = 0, minTwo = 0;
        while(idxOne < len1 && idxTwo < len2){
            int numOne = arrayOne[idxOne];
            int numTwo = arrayTwo[idxTwo];
            int diff = Math.abs(numOne - numTwo);
            if(diff < minDiff){
                minDiff = diff;
                minOne = numOne;
                minTwo = numTwo;
            }
            if(numOne < numTwo){
                idxOne++;
            } else if (numOne > numTwo) {
                idxTwo++;
            } else {
                return new int[]{numOne, numTwo};
            }
        }
        return new int[]{minOne, minTwo};
    }
}
```

