# AlgoExpert

## LinkedLists 13 Node Swap

### Question

Write a function that takes in the head of a Singly Linked List, swaps every pair of adjacent nodes in place (i.e., doesn't create a brand new list), and returns its new head.

If the input Linked List has an odd number of nodes, its final node should remain the same.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

### Sample Input

head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 // the head node with value 0

### Sample Output

1 -> 0 -> 3 -> 2 -> 5 -> 4 // the new head node with value 1

Optimal Space & Time Complexity
O(n) time | O(1) space - where n is the number of nodes in the Linked List

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

  // O(n) time | O(n) space - where n is the number of nodes in the Linked List
  public LinkedList nodeSwap(LinkedList head) {
    if (head == null || head.next == null) {
      return head;
    }

    LinkedList nextNode = head.next;
    head.next = nodeSwap(head.next.next);
    nextNode.next = head;
    return nextNode;
  }
}

```

### Solution 2

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

  // O(n) time | O(1) space - where n is the number of nodes in the Linked List
  public LinkedList nodeSwap(LinkedList head) {
    LinkedList tempNode = new LinkedList(0);
    tempNode.next = head;

    LinkedList prevNode = tempNode;
    while (prevNode.next != null && prevNode.next.next != null) {
      LinkedList firstNode = prevNode.next;
      LinkedList secondNode = prevNode.next.next;
      // prevNode -> firstNode -> secondNode -> x

      firstNode.next = secondNode.next;
      secondNode.next = firstNode;
      prevNode.next = secondNode;
      // prevNode -> secondNode -> firstNode -> x

      prevNode = firstNode;
    }
    return tempNode.next;
  }
}

```
