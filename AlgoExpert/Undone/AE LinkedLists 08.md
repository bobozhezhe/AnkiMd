# AlgoExpert

## LinkedLists 08 Shift Linked List

### Question

Write a function that takes in the head of a Singly Linked List and an integer k, shifts the list in place (i.e., doesn't create a brand new list) by k positions, and returns its new head.

Shifting a Linked List means moving its nodes forward or backward and wrapping them around the list where appropriate. For example, shifting a Linked List forward by one position would make its tail become the new head of the linked list.

Whether nodes are moved forward or backward is determined by whether k is positive or negative.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

### Sample Input

head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 // the head node with value 0
k = 2

### Sample Output

4 -> 5 -> 0 -> 1 -> 2 -> 3 // the new head node with value 4

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the number of nodes in the Linked List

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space - where n is the number of nodes in the Linked List
  public static LinkedList shiftLinkedList(LinkedList head, int k) {
    int listLength = 1;
    LinkedList listTail = head;
    while (listTail.next != null) {
      listTail = listTail.next;
      listLength++;
    }

    int offset = Math.abs(k) % listLength;
    if (offset == 0) return head;
    int newTailPosition = k > 0 ? listLength - offset : offset;
    LinkedList newTail = head;
    for (int i = 1; i < newTailPosition; i++) {
      newTail = newTail.next;
    }

    LinkedList newHead = newTail.next;
    newTail.next = null;
    listTail.next = head;
    return newHead;
  }

  static class LinkedList {
    public int value;
    public LinkedList next;

    public LinkedList(int value) {
      this.value = value;
      next = null;
    }
  }
}

```
