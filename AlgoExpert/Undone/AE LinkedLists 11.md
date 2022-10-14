# AlgoExpert

## LinkedLists 11 Linked List Palindrome

### Question

Write a function that takes in the head of a Singly Linked List and returns a boolean representing whether the linked list's nodes form a palindrome. Your function shouldn't make use of any auxiliary data structure.

A palindrome is usually defined as a string that's written the same forward and backward. For a linked list's nodes to form a palindrome, their values must be the same when read from left to right and from right to left. Note that single-character strings are palindromes, which means that single-node linked lists form palindromes.

Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

You can assume that the input linked list will always have at least one node; in other words, the head will never be None / null.

### Sample Input

head = 0 -> 1 -> 2 -> 2 -> 1 -> 0 // the head node with value 0

### Sample Output

true

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the number of nodes in the Linked List

%

### Key Point

### Solution 1

```java
class Program {

  // O(n) time | O(n) space - where n is the number of nodes in the Linked List
  public boolean linkedListPalindrome(LinkedList head) {
    LinkedListInfo isPalindromeResults = isPalindrome(head, head);
    return isPalindromeResults.outerNodesAreEqual;
  }

  public LinkedListInfo isPalindrome(LinkedList leftNode, LinkedList rightNode) {
    if (rightNode == null) return new LinkedListInfo(true, leftNode);

    LinkedListInfo recursiveCallResults = isPalindrome(leftNode, rightNode.next);
    LinkedList leftNodeToCompare = recursiveCallResults.leftNodeToCompare;
    boolean outerNodesAreEqual = recursiveCallResults.outerNodesAreEqual;

    boolean recursiveIsEqual = outerNodesAreEqual && (leftNodeToCompare.value == rightNode.value);
    LinkedList nextLeftNodeToCompare = leftNodeToCompare.next;

    return new LinkedListInfo(recursiveIsEqual, nextLeftNodeToCompare);
  }

  static class LinkedListInfo {
    public boolean outerNodesAreEqual;
    public LinkedList leftNodeToCompare;

    public LinkedListInfo(boolean outerNodesAreEqual, LinkedList leftNodeToCompare) {
      this.outerNodesAreEqual = outerNodesAreEqual;
      this.leftNodeToCompare = leftNodeToCompare;
    }
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

### Solution 2

```java
class Program {

  // O(n) time | O(1) space - where n is the number of nodes in the Linked List
  public boolean linkedListPalindrome(LinkedList head) {
    LinkedList slowNode = head;
    LinkedList fastNode = head;

    while (fastNode != null && fastNode.next != null) {
      slowNode = slowNode.next;
      fastNode = fastNode.next.next;
    }

    LinkedList reversedSecondHalfNode = reverseLinkedList(slowNode);
    LinkedList firstHalfNode = head;

    while (reversedSecondHalfNode != null) {
      if (reversedSecondHalfNode.value != firstHalfNode.value) return false;
      reversedSecondHalfNode = reversedSecondHalfNode.next;
      firstHalfNode = firstHalfNode.next;
    }

    return true;
  }

  public static LinkedList reverseLinkedList(LinkedList head) {
    LinkedList previousNode = null;
    LinkedList currentNode = head;
    while (currentNode != null) {
      LinkedList nextNode = currentNode.next;
      currentNode.next = previousNode;
      previousNode = currentNode;
      currentNode = nextNode;
    }
    return previousNode;
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
