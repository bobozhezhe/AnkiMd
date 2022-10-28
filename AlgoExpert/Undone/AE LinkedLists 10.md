# AlgoExpert

## LinkedLists 10 Rearrange Linked List

### Question

Write a function that takes in the head of a Singly Linked List and an integer k, rearranges the list in place (i.e., doesn't create a brand new list) around nodes with value k, and returns its new head.

Rearranging a Linked List around nodes with value k means moving all nodes with a value smaller than k before all nodes with value k and moving all nodes with a value greater than k after all nodes with value k.

All moved nodes should maintain their original relative ordering if possible.

Note that the linked list should be rearranged even if it doesn't have any nodes with value k.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

### Sample Input

head = 3 -> 0 -> 5 -> 2 -> 1 -> 4 // the head node with value 3
k = 3

### Sample Output

0 -> 2 -> 1 -> 3 -> 5 -> 4 // the new head node with value 0
// Note that the nodes with values 0, 2, and 1 have
// maintained their original relative ordering, and
// so have the nodes with values 5 and 4.

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the number of nodes in the Linked List

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space - where n is the number of nodes in the Linked List
  public static LinkedList rearrangeLinkedList(LinkedList head, int k) {
    LinkedList smallerListHead = null;
    LinkedList smallerListTail = null;
    LinkedList equalListHead = null;
    LinkedList equalListTail = null;
    LinkedList greaterListHead = null;
    LinkedList greaterListTail = null;

    LinkedList node = head;
    while (node != null) {
      if (node.value < k) {
        LinkedListPair smallerList = growLinkedList(smallerListHead, smallerListTail, node);
        smallerListHead = smallerList.head;
        smallerListTail = smallerList.tail;
      } else if (node.value > k) {
        LinkedListPair greaterList = growLinkedList(greaterListHead, greaterListTail, node);
        greaterListHead = greaterList.head;
        greaterListTail = greaterList.tail;
      } else {
        LinkedListPair equalList = growLinkedList(equalListHead, equalListTail, node);
        equalListHead = equalList.head;
        equalListTail = equalList.tail;
      }

      LinkedList prevNode = node;
      node = node.next;
      prevNode.next = null;
    }

    LinkedListPair firstPair =
        connectLinkedLists(smallerListHead, smallerListTail, equalListHead, equalListTail);
    LinkedListPair finalPair =
        connectLinkedLists(firstPair.head, firstPair.tail, greaterListHead, greaterListTail);
    return finalPair.head;
  }

  public static LinkedListPair growLinkedList(LinkedList head, LinkedList tail, LinkedList node) {
    LinkedList newHead = head;
    LinkedList newTail = node;

    if (newHead == null) newHead = node;
    if (tail != null) tail.next = node;

    return new LinkedListPair(newHead, newTail);
  }

  public static LinkedListPair connectLinkedLists(
      LinkedList headOne, LinkedList tailOne, LinkedList headTwo, LinkedList tailTwo) {
    LinkedList newHead = headOne == null ? headTwo : headOne;
    LinkedList newTail = tailTwo == null ? tailOne : tailTwo;

    if (tailOne != null) tailOne.next = headTwo;

    return new LinkedListPair(newHead, newTail);
  }

  static class LinkedListPair {
    public LinkedList head;
    public LinkedList tail;

    public LinkedListPair(LinkedList head, LinkedList tail) {
      this.head = head;
      this.tail = tail;
    }
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
