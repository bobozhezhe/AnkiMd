# AlgoExpert

## Graphs 03 Breadth-first Search

### Question

You're given a Node class that has a name and an array of optional children nodes. When put together, nodes form an acyclic tree-like structure.

Implement the breadthFirstSearch method on the Node class, which takes in an empty array, traverses the tree using the Breadth-first Search approach (specifically navigating the tree from left to right), stores all of the nodes' names in the input array, and returns it.

If you're unfamiliar with Breadth-first Search, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

graph = A
     /  |  \
    B   C   D
   / \     / \
  E   F   G   H
     / \   \
    I   J   K

### Sample Output

["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K"]

### Optimal Space & Time Complexity

O(v + e) time | O(v) space - where v is the number of vertices of the input graph and e is the number of edges of the input graph

%

### Key Point

### Solution 1

```java
class Program {
  static class Node {
    String name;
    List<Node> children = new ArrayList<Node>();

    public Node(String name) {
      this.name = name;
    }

    // O(v + e) time | O(v) space
    public List<String> breadthFirstSearch(List<String> array) {
      Queue<Node> queue = new LinkedList<Node>();
      queue.add(this);
      while (!queue.isEmpty()) {
        Node current = queue.poll();
        array.add(current.name);
        queue.addAll(current.children);
      }
      return array;
    }

    public Node addChild(String name) {
      Node child = new Node(name);
      children.add(child);
      return this;
    }
  }
}

```
