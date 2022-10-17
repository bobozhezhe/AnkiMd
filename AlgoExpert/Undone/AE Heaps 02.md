# AlgoExpert

## Heaps 02 Continuous Median

### Question

Write a ContinuousMedianHandler class that supports:

The continuous insertion of numbers with the insert method.
The instant (O(1) time) retrieval of the median of the numbers that have been inserted thus far with the getMedian method.
The getMedian method has already been written for you. You simply have to write the insert method.

The median of a set of numbers is the "middle" number when the numbers are ordered from smallest to greatest. If there's an odd number of numbers in the set, as in {1, 3, 7}, the median is the number in the middle (3 in this case); if there's an even number of numbers in the set, as in {1, 3, 7, 8}, the median is the average of the two middle numbers ((3 + 7) / 2 == 5 in this case).

### Sample Usage

// All operations below are performed sequentially.
ContinuousMedianHandler(): - // instantiate a ContinuousMedianHandler
insert(5): -
insert(10): -
getMedian(): 7.5
insert(100): -
getMedian(): 10

### Optimal Space & Time Complexity

Insert: O(log(n)) time | O(n) space - where n is the number of inserted numbers

%

### Key Point

### Solution 1

```java
class Program {
          }
        } else {
          idxToSwap = childOneIdx;
        }
        if (comparisonFunc.apply(heap.get(idxToSwap), heap.get(currentIdx))) {
          swap(currentIdx, idxToSwap, heap);
          currentIdx = idxToSwap;
          childOneIdx = currentIdx * 2 + 1;
        } else {
          return;
        }
      }
    }

    public void siftUp(int currentIdx, List<Integer> heap) {
      int parentIdx = (currentIdx - 1) / 2;
      while (currentIdx > 0) {
        if (comparisonFunc.apply(heap.get(currentIdx), heap.get(parentIdx))) {
          swap(currentIdx, parentIdx, heap);
          currentIdx = parentIdx;
          parentIdx = (currentIdx - 1) / 2;
        } else {
          return;
        }
      }
    }

    public int peek() {
      return heap.get(0);
    }

    public int remove() {
      swap(0, heap.size() - 1, heap);
      int valueToRemove = heap.get(heap.size() - 1);
      heap.remove(heap.size() - 1);
      length--;
      siftDown(0, heap.size() - 1, heap);
      return valueToRemove;
    }

    public void insert(int value) {
      heap.add(value);
      length++;
      siftUp(heap.size() - 1, heap);
    }

    public void swap(int i, int j, List<Integer> heap) {
      Integer temp = heap.get(j);
      heap.set(j, heap.get(i));
      heap.set(i, temp);
    }

    public static Boolean MAX_HEAP_FUNC(Integer a, Integer b) {
      return a > b;
    }

    public static Boolean MIN_HEAP_FUNC(Integer a, Integer b) {
      return a < b;
    }
  }
}

```
