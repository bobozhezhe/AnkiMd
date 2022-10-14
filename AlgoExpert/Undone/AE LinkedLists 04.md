# AlgoExpert

## LinkedLists 04 Sum of Linked Lists

### Question

You're given two Linked Lists of potentially unequal length. Each Linked List represents a non-negative integer, where each node in the Linked List is a digit of that integer, and the first node in each Linked List always represents the least significant digit of the integer. Write a function that returns the head of a new Linked List that represents the sum of the integers represented by the two input Linked Lists.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list. The value of each LinkedList node is always in the range of 0 - 9.

Note: your function must create and return a new Linked List, and you're not allowed to modify either of the input Linked Lists.

### Sample Input

linkedListOne = 2 -> 4 -> 7 -> 1
linkedListTwo = 9 -> 4 -> 5

### Sample Output

1 -> 9 -> 2 -> 2
// linkedListOne represents 1742
// linkedListTwo represents 549
// 1742 + 549 = 2291

### Optimal Space & Time Complexity

O(max(n, m)) time | O(max(n, m)) space - where n is the length of the first Linked List and m is the length of the second Linked List

%

### Key Point

### Solution 1

```java
class Program {
  // This is an input class. Do not edit.
  public static class LinkedList {
    public int value;
    public LinkedList next;

    public LinkedList(int value) {
      this.value = value;
      this.next = null;
    }
  }
  // O(max(n, m)) time | O(max(n, m)) space - where n is the length of the
  // first Linked List and m is the length of the second Linked List
  public LinkedList sumOfLinkedLists(LinkedList linkedListOne, LinkedList linkedListTwo) {
    // This variable will store a dummy node whose .next
    // attribute will point to the head of our new LL.
    LinkedList newLinkedListHeadPointer = new LinkedList(0);
    LinkedList currentNode = newLinkedListHeadPointer;
    int carry = 0;

    LinkedList nodeOne = linkedListOne;
    LinkedList nodeTwo = linkedListTwo;

    while (nodeOne != null || nodeTwo != null || carry != 0) {
      int valueOne = (nodeOne != null) ? nodeOne.value : 0;
      int valueTwo = (nodeTwo != null) ? nodeTwo.value : 0;
      int sumOfValues = valueOne + valueTwo + carry;

      int newValue = sumOfValues % 10;
      LinkedList newNode = new LinkedList(newValue);
      currentNode.next = newNode;
      currentNode = newNode;

      carry = sumOfValues / 10;
      nodeOne = (nodeOne != null) ? nodeOne.next : null;
      nodeTwo = (nodeTwo != null) ? nodeTwo.next : null;
    }

    return newLinkedListHeadPointer.next;
  }
}

```
