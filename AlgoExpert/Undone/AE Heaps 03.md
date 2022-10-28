# AlgoExpert

## Heaps 03 Sort K-Sorted Array

### Question

Write a function that takes in a non-negative integer k and a k-sorted array of integers and returns the sorted version of the array. Your function can either sort the array in place or create an entirely new array.

A k-sorted array is a partially sorted array in which all elements are at most k positions away from their sorted position. For example, the array [3, 1, 2, 2] is k-sorted with k = 3, because each element in the array is at most 3 positions away from its sorted position.

Note that you're expected to come up with an algorithm that can sort the k-sorted array faster than in O(nlog(n)) time.

### Sample Input

array = [3, 2, 1, 5, 4, 7, 6, 5]
k = 3

### Sample Output

[1, 2, 3, 4, 5, 5, 6, 7]

### Optimal Space & Time Complexity

O(nlog(k)) time | O(k) space - where n is the number of elements in the array and k is how far away elements are from their sorted position

%

### Key Point

### Solution 1

```java
class Program {

  // O(nlog(k)) time | O(k) space - where n is the number
  // of elements in the array and k is how far away elements
  // are from their sorted position
  public int[] sortKSortedArray(int[] array, int k) {
    List<Integer> heapValues = new ArrayList<Integer>();
    for (int i = 0; i < Math.min(k + 1, array.length); i++) heapValues.add(array[i]);

    MinHeap minHeapWithKElements = new MinHeap(heapValues);

    int nextIndexToInsertElement = 0;
    for (int idx = k + 1; idx < array.length; idx++) {
      int minElement = minHeapWithKElements.remove();
      array[nextIndexToInsertElement] = minElement;
      nextIndexToInsertElement += 1;

      int currentElement = array[idx];
      minHeapWithKElements.insert(currentElement);
    }

    while (!minHeapWithKElements.isEmpty()) {
      int minElement = minHeapWithKElements.remove();
      array[nextIndexToInsertElement] = minElement;
      nextIndexToInsertElement += 1;
    }

    return array;
  }

  class MinHeap {
    List<Integer> heap = new ArrayList<Integer>();

    public MinHeap(List<Integer> array) {
      heap = buildHeap(array);
    }

    // O(n) time | O(1) space
    public List<Integer> buildHeap(List<Integer> array) {
      int firstParentIdx = (array.size() - 2) / 2;
      for (int currentIdx = firstParentIdx; currentIdx >= 0; currentIdx--) {
        siftDown(currentIdx, array.size() - 1, array);
      }
      return array;
    }

    // O(log(n)) time | O(1) space
    public void siftDown(int currentIdx, int endIdx, List<Integer> heap) {
      int childOneIdx = currentIdx * 2 + 1;
      while (childOneIdx <= endIdx) {
        int childTwoIdx = currentIdx * 2 + 2 <= endIdx ? currentIdx * 2 + 2 : -1;
        int idxToSwap;
        if (childTwoIdx != -1 && heap.get(childTwoIdx) < heap.get(childOneIdx)) {
          idxToSwap = childTwoIdx;
        } else {
          idxToSwap = childOneIdx;
        }
        if (heap.get(idxToSwap) < heap.get(currentIdx)) {
          swap(currentIdx, idxToSwap, heap);
          currentIdx = idxToSwap;
          childOneIdx = currentIdx * 2 + 1;
        } else {
          return;
        }
      }
    }

    // O(log(n)) time | O(1) space
    public void siftUp(int currentIdx, List<Integer> heap) {
      int parentIdx = (currentIdx - 1) / 2;
      while (currentIdx > 0 && heap.get(currentIdx) < heap.get(parentIdx)) {
        swap(currentIdx, parentIdx, heap);
        currentIdx = parentIdx;
        parentIdx = (currentIdx - 1) / 2;
      }
    }

    public int peek() {
      return heap.get(0);
    }

    public int remove() {
      swap(0, heap.size() - 1, heap);
      int valueToRemove = heap.get(heap.size() - 1);
      heap.remove(heap.size() - 1);
      siftDown(0, heap.size() - 1, heap);
      return valueToRemove;
    }

    public void insert(int value) {
      heap.add(value);
      siftUp(heap.size() - 1, heap);
    }

    public void swap(int i, int j, List<Integer> heap) {
      Integer temp = heap.get(j);
      heap.set(j, heap.get(i));
      heap.set(i, temp);
    }

    public boolean isEmpty() {
      return heap.size() == 0;
    }
  }
}

```
