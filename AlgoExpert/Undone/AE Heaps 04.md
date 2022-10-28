# AlgoExpert

## Heaps 04 Laptop Rentals

### Question

You're given a list of time intervals during which students at a school need a laptop. These time intervals are represented by pairs of integers [start, end], where 0 <= start < end. However, start and end don't represent real times; therefore, they may be greater than 24.

No two students can use a laptop at the same time, but immediately after a student is done using a laptop, another student can use that same laptop. For example, if one student rents a laptop during the time interval [0, 2], another student can rent the same laptop during any time interval starting with 2.

Write a function that returns the minimum number of laptops that the school needs to rent such that all students will always have access to a laptop when they need one.

### Sample Input

times =
[
  [0, 2],
  [1, 4],
  [4, 6],
  [0, 4],
  [7, 8],
  [9, 11],
  [3, 10],
]

### Sample Output

3

### Optimal Space & Time Complexity

O(nlog(n)) time | O(n) space - where n is the number of times

%

### Key Point

### Solution 1

```java
class Program {

  // O(nlog(n)) time | O(n) space - where n is the number of times
  public int laptopRentals(ArrayList<ArrayList<Integer>> times) {
    if (times.size() == 0) {
      return 0;
    }

    Collections.sort(times, (a, b) -> Integer.compare(a.get(0), b.get(0)));

    ArrayList<ArrayList<Integer>> timesWhenLaptopIsUsed = new ArrayList<ArrayList<Integer>>();
    timesWhenLaptopIsUsed.add(times.get(0));
    MinHeap heap = new MinHeap(timesWhenLaptopIsUsed);

    for (int idx = 1; idx < times.size(); idx++) {
      ArrayList<Integer> currentInterval = times.get(idx);
      if (heap.peek().get(1) <= currentInterval.get(0)) {
        heap.remove();
      }
      heap.insert(currentInterval);
    }

    return timesWhenLaptopIsUsed.size();
  }

  static class MinHeap {
    ArrayList<ArrayList<Integer>> heap = new ArrayList<ArrayList<Integer>>();

    public MinHeap(ArrayList<ArrayList<Integer>> array) {
      heap = buildHeap(array);
    }

    public ArrayList<ArrayList<Integer>> buildHeap(ArrayList<ArrayList<Integer>> array) {
      int firstParentIdx = (array.size() - 2) / 2;
      for (int currentIdx = firstParentIdx; currentIdx >= 0; currentIdx--) {
        siftDown(currentIdx, array.size() - 1, array);
      }
      return array;
    }

    public void siftDown(int currentIdx, int endIdx, ArrayList<ArrayList<Integer>> heap) {
      int newCurrentIdx = currentIdx;
      int childOneIdx = currentIdx * 2 + 1;
      while (childOneIdx <= endIdx) {
        int childTwoIdx = (newCurrentIdx * 2 + 2 <= endIdx) ? newCurrentIdx * 2 + 2 : -1;
        int idxToSwap;
        if (childTwoIdx != -1 && heap.get(childTwoIdx).get(1) < heap.get(childOneIdx).get(1)) {
          idxToSwap = childTwoIdx;
        } else {
          idxToSwap = childOneIdx;
        }
        if (heap.get(idxToSwap).get(1) < heap.get(currentIdx).get(1)) {
          swap(newCurrentIdx, idxToSwap, heap);
          newCurrentIdx = idxToSwap;
          childOneIdx = newCurrentIdx * 2 + 1;
        } else {
          return;
        }
      }
    }

    public void siftUp(int currentIdx, ArrayList<ArrayList<Integer>> heap) {
      int newCurrentIdx = currentIdx;
      int parentIdx = (currentIdx - 1) / 2;
      while (newCurrentIdx > 0 && heap.get(newCurrentIdx).get(1) < heap.get(parentIdx).get(1)) {
        swap(newCurrentIdx, parentIdx, heap);
        newCurrentIdx = parentIdx;
        parentIdx = (newCurrentIdx - 1) / 2;
      }
    }

    public ArrayList<Integer> peek() {
      return heap.get(0);
    }

    public ArrayList<Integer> remove() {
      swap(0, heap.size() - 1, heap);
      ArrayList<Integer> valueToRemove = heap.get(heap.size() - 1);
      heap.remove(heap.size() - 1);
      siftDown(0, heap.size() - 1, heap);
      return valueToRemove;
    }

    public void insert(ArrayList<Integer> value) {
      heap.add(value);
      siftUp(heap.size() - 1, heap);
    }

    public void swap(int i, int j, ArrayList<ArrayList<Integer>> heap) {
      ArrayList<Integer> temp = heap.get(j);
      heap.set(j, heap.get(i));
      heap.set(i, temp);
    }
  }
}

```

### Solution 2

```java
class Program {

  // O(nlog(n)) time | O(n) space - where n is the number of times
  public int laptopRentals(ArrayList<ArrayList<Integer>> times) {
    if (times.size() == 0) {
      return 0;
    }

    int usedLaptops = 0;
    ArrayList<Integer> startTimes = new ArrayList<Integer>();
    ArrayList<Integer> endTimes = new ArrayList<Integer>();

    for (ArrayList<Integer> interval : times) {
      startTimes.add(interval.get(0));
      endTimes.add((interval.get(1)));
    }

    Collections.sort(startTimes);
    Collections.sort(endTimes);

    int startIterator = 0;
    int endIterator = 0;

    while (startIterator < times.size()) {
      if (startTimes.get(startIterator) >= endTimes.get(endIterator)) {
        usedLaptops -= 1;
        endIterator += 1;
      }

      usedLaptops += 1;
      startIterator += 1;
    }

    return usedLaptops;
  }
}

```
