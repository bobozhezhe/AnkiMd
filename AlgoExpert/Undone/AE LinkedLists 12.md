# AlgoExpert

## LinkedLists 12 Zip Linked List

### Question

You're given the head of a Singly Linked List of arbitrary length k. Write a function that zips the Linked List in place (i.e., doesn't create a brand new list) and returns its head.

A Linked List is zipped if its nodes are in the following order, where k is the length of the Linked List:

1st node -> kth node -> 2nd node -> (k - 1)th node -> 3rd node -> (k - 2)th node -> ...
Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

### Sample Input

linkedList = 1 -> 2 -> 3 -> 4 -> 5 -> 6 // the head node with value 1

### Sample Output

1 -> 6 -> 2 -> 5 -> 3 -> 4 // the head node with value 1

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the length of the input Linked List

%

### Key Point

### Solution 1

```java
class Program {
      this.next = null;
    }
  }

  // O(n) time | O(1) space - where n is the length of the Linked List
  public LinkedList zipLinkedList(LinkedList linkedList) {
    if (linkedList.next == null || linkedList.next.next == null) {
      return linkedList;
    }

    LinkedList firstHalfHead = linkedList;
    LinkedList secondHalfHead = splitLinkedList(linkedList);

    LinkedList reversedSecondHalfHead = reverseLinkedList(secondHalfHead);

    return interweaveLinkedLists(firstHalfHead, reversedSecondHalfHead);
  }

  public LinkedList splitLinkedList(LinkedList linkedList) {
    LinkedList slowIterator = linkedList;
    LinkedList fastIterator = linkedList;

    while (fastIterator != null && fastIterator.next != null) {
      slowIterator = slowIterator.next;
      fastIterator = fastIterator.next.next;
    }

    LinkedList secondHalfHead = slowIterator.next;
    slowIterator.next = null;
    return secondHalfHead;
  }

  public LinkedList interweaveLinkedLists(LinkedList linkedList1, LinkedList linkedList2) {
    LinkedList linkedList1Iterator = linkedList1;
    LinkedList linkedList2Iterator = linkedList2;

    while (linkedList1Iterator != null && linkedList2Iterator != null) {
      LinkedList firstHalfIteratorNext = linkedList1Iterator.next;
      LinkedList secondHalfIteratorNext = linkedList2Iterator.next;

      linkedList1Iterator.next = linkedList2Iterator;
      linkedList2Iterator.next = firstHalfIteratorNext;

      linkedList1Iterator = firstHalfIteratorNext;
      linkedList2Iterator = secondHalfIteratorNext;
    }

    return linkedList1;
  }

  public LinkedList reverseLinkedList(LinkedList linkedList) {
    LinkedList previousNode = null;
    LinkedList currentNode = linkedList;
    while (currentNode != null) {
      LinkedList nextNode = currentNode.next;
      currentNode.next = previousNode;
      previousNode = currentNode;
      currentNode = nextNode;
    }
    return previousNode;
  }
}

```
