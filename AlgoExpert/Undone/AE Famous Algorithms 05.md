# AlgoExpert

## Famous Algorithms 05 A* Algorithm

### Question

You're given a two-dimensional array containing 0s and 1s, where each 0 represents a free space and each 1 represents an obstacle (a space that cannot be passed through). You can think of this array as a grid-shaped graph. You're also given four integers startRow, startCol, endRow, and endCol, representing the positions of a start node and an end node in the graph.

Write a function that finds the shortest path between the start node and the end node using the A* search algorithm and returns it.

The shortest path should be returned as an array of node positions, where each node position is an array of two elements: the [row, col] of the respective node in the graph. The output array should contain the start node's position, the end node's position, and all of the positions of the remaining nodes in the shortest path, and these node positions should be ordered from start node to end node.

If there is no path from the start node to the end node, your function should return an empty array.

Note that:

From each node in the graph, you can only travel in four directions: up, left, down and right; you can't travel diagonally.
The distance between all neighboring nodes in the graph is the same; you can treat it as a distance of 1.
The start node and end node are guaranteed to be located in empty spaces (cells containing 0).
The start node and end node will never be out of bounds and will never overlap.
There will be at most one shortest path from the start node to the end node.
If you're unfamiliar with A*, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

startRow = 0
startCol = 1
endRow = 4
endCol = 3
graph = [
  [0, 0, 0, 0, 0],
  [0, 1, 1, 1, 0],
  [0, 0, 0, 0, 0],
  [1, 0, 1, 1, 1],
  [0, 0, 0, 0, 0],
]

### Sample Output

[[0, 1], [0, 0], [1, 0], [2, 0], [2, 1], [3, 1], [4, 1], [4, 2], [4, 3]]

// The shortest path can be clearly seen here:
// [
//   [., ., 0, 0, 0],
//   [., 1, 1, 1, 0],
//   [., ., 0, 0, 0],
//   [1, ., 1, 1, 1],
//   [0, ., ., ., 0],
// ]

### Optimal Space & Time Complexity

O(w *h* log(w *h)) time | O(w* h) space - where w is the width of the graph and h is the height

%

### Key Point

### Solution 1

```java
class Program {

  class Node {
    String id;
    int row;
    int col;
    int value;
    int distanceFromStart;
    int estimatedDistanceToEnd;
    Node cameFrom;

    Node(int row, int col, int value) {
      this.id = String.valueOf(row) + '-' + String.valueOf(col);
      this.row = row;
      this.col = col;
      this.value = value;
      this.distanceFromStart = Integer.MAX_VALUE;
      this.estimatedDistanceToEnd = Integer.MAX_VALUE;
      this.cameFrom = null;
    }
  };

  class MinHeap {
    List<Node> heap = new ArrayList<Node>();
    Map<String, Integer> nodePositionsInHeap = new HashMap<String, Integer>();

    public MinHeap(List<Node> array) {
      for (int i = 0; i < array.size(); i++) {
        Node node = array.get(i);
        nodePositionsInHeap.put(node.id, i);
      }
      heap = buildHeap(array);
    }

    // O(n) time | O(1) space
    List<Node> buildHeap(List<Node> array) {
      int firstParentIdx = (array.size() - 2) / 2;
      for (int currentIdx = firstParentIdx + 1; currentIdx >= 0; currentIdx--) {
        siftDown(currentIdx, array.size() - 1, array);
      }
      return array;
    }

    boolean isEmpty() {
      return heap.size() == 0;
    }

    // O(log(n)) time | O(1) space
    void siftDown(int currentIdx, int endIdx, List<Node> array) {
      int childOneIdx = currentIdx * 2 + 1;
      while (childOneIdx <= endIdx) {
        int childTwoIdx = currentIdx * 2 + 2 <= endIdx ? currentIdx * 2 + 2 : -1;
        int idxToSwap;
        if (childTwoIdx != -1
            && array.get(childTwoIdx).estimatedDistanceToEnd
                < heap.get(childOneIdx).estimatedDistanceToEnd) {
          idxToSwap = childTwoIdx;
        } else {
          idxToSwap = childOneIdx;
        }
        if (array.get(idxToSwap).estimatedDistanceToEnd
            < array.get(currentIdx).estimatedDistanceToEnd) {
          swap(currentIdx, idxToSwap);
          currentIdx = idxToSwap;
          childOneIdx = currentIdx * 2 + 1;
        } else {
          return;
        }
      }
    }

    // O(log(n)) time | O(1) space
    void siftUp(int currentIdx) {
      int parentIdx = (currentIdx - 1) / 2;
      while (currentIdx > 0
          && heap.get(currentIdx).estimatedDistanceToEnd
              < heap.get(parentIdx).estimatedDistanceToEnd) {
        swap(currentIdx, parentIdx);
        currentIdx = parentIdx;
        parentIdx = (currentIdx - 1) / 2;
      }
    }

    Node remove() {
      if (isEmpty()) {
        return null;
      }

      swap(0, heap.size() - 1);
      Node node = heap.get(heap.size() - 1);
      heap.remove(heap.size() - 1);
      nodePositionsInHeap.remove(node.id);
      siftDown(0, heap.size() - 1, heap);
      return node;
    }

    void insert(Node node) {
      heap.add(node);
      nodePositionsInHeap.put(node.id, heap.size() - 1);
      siftUp(heap.size() - 1);
    }

    void swap(int i, int j) {
      nodePositionsInHeap.put(heap.get(i).id, j);
      nodePositionsInHeap.put(heap.get(j).id, i);
      Node temp = heap.get(i);
      heap.set(i, heap.get(j));
      heap.set(j, temp);
    }

    boolean containsNode(Node node) {
      return nodePositionsInHeap.containsKey(node.id);
    }

    void update(Node node) {
      siftUp(nodePositionsInHeap.get(node.id));
    }
  };


  // O(w * h * log(w * h)) time | O(w * h) space - where
  // w is the width of the graph and h is the height
  public int[][] aStarAlgorithm(int startRow, int startCol, int endRow, int endCol, int[][] graph) {
    List<List<Node>> nodes = initializeNodes(graph);
    Node startNode = nodes.get(startRow).get(startCol);
    Node endNode = nodes.get(endRow).get(endCol);

    startNode.distanceFromStart = 0;
    startNode.estimatedDistanceToEnd = calculateManhattanDistance(startNode, endNode);

    List<Node> nodesToVisitList = new ArrayList<Node>();
    nodesToVisitList.add(startNode);
    MinHeap nodesToVisit = new MinHeap(nodesToVisitList);

    while (!nodesToVisit.isEmpty()) {
      Node currentMinDistanceNode = nodesToVisit.remove();

      if (currentMinDistanceNode == endNode) {
        break;
      }

      List<Node> neighbors = getNeightboringNodes(currentMinDistanceNode, nodes);

      for (Node neighbor : neighbors) {
        if (neighbor.value == 1) {
          continue;
        }

        int tentativeDistanceToNeighbor = currentMinDistanceNode.distanceFromStart + 1;

        if (tentativeDistanceToNeighbor >= neighbor.distanceFromStart) {
          continue;
        }

        neighbor.cameFrom = currentMinDistanceNode;
        neighbor.distanceFromStart = tentativeDistanceToNeighbor;
        neighbor.estimatedDistanceToEnd =
            tentativeDistanceToNeighbor + calculateManhattanDistance(neighbor, endNode);

        if (!nodesToVisit.containsNode(neighbor)) {
          nodesToVisit.insert(neighbor);
        } else {
          nodesToVisit.update(neighbor);
        }
      }
    }

    return reconstructPath(endNode);
  }

  List<List<Node>> initializeNodes(int[][] graph) {
    List<List<Node>> nodes = new ArrayList<List<Node>>();

    for (int i = 0; i < graph.length; i++) {
      List<Node> nodeList = new ArrayList<Node>();
      nodes.add(nodeList);
      for (int j = 0; j < graph[i].length; j++) {
        nodes.get(i).add(new Node(i, j, graph[i][j]));
      }
    }

    return nodes;
  }

  int calculateManhattanDistance(Node currentNode, Node endNode) {
    int currentRow = currentNode.row;
    int currentCol = currentNode.col;
    int endRow = endNode.row;
    int endCol = endNode.col;

    return Math.abs(currentRow - endRow) + Math.abs(currentCol - endCol);
  }

  List<Node> getNeightboringNodes(Node node, List<List<Node>> nodes) {
    List<Node> neighbors = new ArrayList<Node>();

    int numRows = nodes.size();
    int numCols = nodes.get(0).size();

    int row = node.row;
    int col = node.col;

    if (row < numRows - 1) { // DOWN
      neighbors.add(nodes.get(row + 1).get(col));
    }

    if (row > 0) { // UP
      neighbors.add(nodes.get(row - 1).get(col));
    }

    if (col < numCols - 1) { // RIGHT
      neighbors.add(nodes.get(row).get(col + 1));
    }

    if (col > 0) { // LEFT
      neighbors.add(nodes.get(row).get(col - 1));
    }

    return neighbors;
  }

  int[][] reconstructPath(Node endNode) {
    if (endNode.cameFrom == null) {
      return new int[][] {};
    }

    Node currentNode = endNode;
    List<List<Integer>> path = new ArrayList<List<Integer>>();

    while (currentNode != null) {
      List<Integer> nodeData = new ArrayList<Integer>();
      nodeData.add(currentNode.row);
      nodeData.add(currentNode.col);
      path.add(nodeData);
      currentNode = currentNode.cameFrom;
    }

    // convert path to return type int[][] and reverse
    int[][] res = new int[path.size()][2];
    for (int i = 0; i < res.length; i++) {
      res[i][0] = path.get(res.length - 1 - i).get(0);
      res[i][1] = path.get(res.length - 1 - i).get(1);
    }

    return res;
  }
}
```
