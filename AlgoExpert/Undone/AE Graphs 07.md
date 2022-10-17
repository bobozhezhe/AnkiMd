# AlgoExpert

## Graphs 08 Cycle In Graph

### Question

You're given a list of edges representing an unweighted, directed graph with at least one node. Write a function that returns a boolean representing whether the given graph contains a cycle.

For the purpose of this question, a cycle is defined as any number of vertices, including just one vertex, that are connected in a closed chain. A cycle can also be defined as a chain of at least one vertex in which the first vertex is the same as the last.

The given list is what's called an adjacency list, and it represents a graph. The number of vertices in the graph is equal to the length of edges, where each index i in edges contains vertex i's outbound edges, in no particular order. Each individual edge is represented by a positive integer that denotes an index (a destination vertex) in the list that this vertex is connected to. Note that these edges are directed, meaning that you can only travel from a particular vertex to its destination, not the other way around (unless the destination vertex itself has an outbound edge to the original vertex).

Also note that this graph may contain self-loops. A self-loop is an edge that has the same destination and origin; in other words, it's an edge that connects a vertex to itself. For the purpose of this question, a self-loop is considered a cycle.

For a more detailed explanation, please refer to the Conceptual Overview section of this question's video explanation.

### Sample Input

edges = [
  [1, 3],
  [2, 3, 4],
  [0],
  [],
  [2, 5],
  [],
]

### Sample Output

true
// There are multiple cycles in this graph:
// 1) 0 -> 1 -> 2 -> 0
// 2) 0 -> 1 -> 4 -> 2 -> 0
// 3) 1 -> 2 -> 0 -> 1
// These are just 3 examples; there are more.

### Optimal Space & Time Complexity

O(v + e) time | O(v) space - where v is the number of vertices and e is the number of edges in the graph

%

### Key Point

### Solution 1

```java
class Program {

  // O(v + e) time | O(v) space - where v is the number of
  // vertices and e is the number of edges in the graph
  public boolean cycleInGraph(int[][] edges) {
    int numberOfNodes = edges.length;
    boolean[] visited = new boolean[numberOfNodes];
    boolean[] currentlyInStack = new boolean[numberOfNodes];
    Arrays.fill(visited, false);
    Arrays.fill(currentlyInStack, false);

    for (int node = 0; node < numberOfNodes; node++) {
      if (visited[node]) {
        continue;
      }

      boolean containsCycle = isNodeInCycle(node, edges, visited, currentlyInStack);
      if (containsCycle) {
        return true;
      }
    }

    return false;
  }

  public boolean isNodeInCycle(
      int node, int[][] edges, boolean[] visited, boolean[] currentlyInStack) {
    visited[node] = true;
    currentlyInStack[node] = true;

    boolean containsCycle = false;
    int[] neighbors = edges[node];
    for (int neighbor : neighbors) {
      if (!visited[neighbor]) {
        containsCycle = isNodeInCycle(neighbor, edges, visited, currentlyInStack);
      }
      if (containsCycle) {
        return true;
      } else if (currentlyInStack[neighbor]) {
        return true;
      }
    }

    currentlyInStack[node] = false;
    return false;
  }
}

```

### Solution 2

```java
class Program {

  public int WHITE = 0;
  public int GREY = 1;
  public int BLACK = 3;

  // O(v + e) time | O(v) space - where v is the number of
  // vertices and e is the number of edges in the graph
  public boolean cycleInGraph(int[][] edges) {
    int numberOfNodes = edges.length;
    int[] colors = new int[numberOfNodes];
    Arrays.fill(colors, WHITE);

    for (int node = 0; node < numberOfNodes; node++) {
      if (colors[node] != WHITE) continue;

      boolean containsCycle = traverseAndColorNodes(node, edges, colors);
      if (containsCycle) return true;
    }

    return false;
  }

  public boolean traverseAndColorNodes(int node, int[][] edges, int[] colors) {
    colors[node] = GREY;

    int[] neighbors = edges[node];
    for (int neighbor : neighbors) {
      int neighborColor = colors[neighbor];

      if (neighborColor == GREY) {
        return true;
      }

      if (neighborColor == BLACK) {
        continue;
      }

      boolean containsCycle = traverseAndColorNodes(neighbor, edges, colors);
      if (containsCycle) {
        return true;
      }
    }

    colors[node] = BLACK;
    return false;
  }
}

```
