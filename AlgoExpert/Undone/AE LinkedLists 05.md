# AlgoExpert

## LinkedLists 05 Find Loop

### Question

Write a function that takes in the head of a Singly Linked List that contains a loop (in other words, the list's tail node points to some node in the list instead of None / null). The function should return the node (the actual node--not just its value) from which the loop originates in constant space.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list.

### Sample Input

head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 // the head node with value 0
                           ^         v
                           9 <- 8 <- 7

### Sample Output

4 -> 5 -> 6 // the node with value 4
^         v
9 <- 8 <- 7

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the number of nodes in the Linked List

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(1) space
  public static LinkedList findLoop(LinkedList head) {
    LinkedList first = head.next;
    LinkedList second = head.next.next;
    while (first != second) {
      first = first.next;
      second = second.next.next;
    }
    first = head;
    while (first != second) {
      first = first.next;
      second = second.next;
    }
    return first;
  }

  static class LinkedList {
    int value;
    LinkedList next = null;

    public LinkedList(int value) {
      this.value = value;
    }
  }
}

```
