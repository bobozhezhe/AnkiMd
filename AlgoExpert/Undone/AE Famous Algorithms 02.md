# AlgoExpert

## Famous Algorithms 02 Dijkstra's Algorithm

### Question

You're given an integer start and a list edges of pairs of integers.

The list is what's called an adjacency list, and it represents a graph. The number of vertices in the graph is equal to the length of edges, where each index i in edges contains vertex i's outbound edges, in no particular order. Each individual edge is represented by an pair of two numbers, [destination, distance], where the destination is a positive integer denoting the destination vertex and the distance is a positive integer representing the length of the edge (the distance from vertex i to vertex destination). Note that these edges are directed, meaning that you can only travel from a particular vertex to its destination—not the other way around (unless the destination vertex itself has an outbound edge to the original vertex).

Write a function that computes the lengths of the shortest paths between start and all of the other vertices in the graph using Dijkstra's algorithm and returns them in an array. Each index i in the output array should represent the length of the shortest path between start and vertex i. If no path is found from start to vertex i, then output[i] should be -1.

Note that the graph represented by edges won't contain any self-loops (vertices that have an outbound edge to themselves) and will only have positively weighted edges (i.e., no negative distances).

If you're unfamiliar with Dijkstra's algorithm, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input

start = 0
edges = [
  [[1, 7]],
  [[2, 6], [3, 20], [4, 3]],
  [[3, 14]],
  [[4, 2]],
  [],
  [],
]

### Sample Output

[0, 7, 13, 27, 10, -1]

### Optimal Space & Time Complexity

O((v + e) * log(v)) time | O(v) space - where v is the number of vertices and e is the number of edges in the input graph

%

### Key Point

### Solution 1

```java
class Program {
  // O(v^2 + e) time | O(v) space - where v is the number of
  // vertices and e is the number of edges in the input graph
  public int[] dijkstrasAlgorithm(int start, int[][][] edges) {
    int numberOfVertices = edges.length;

    int[] minDistances = new int[edges.length];
    Arrays.fill(minDistances, Integer.MAX_VALUE);
    minDistances[start] = 0;

    Set<Integer> visited = new HashSet<Integer>();

    while (visited.size() != numberOfVertices) {
      int[] getVertexData = getVertexWithMinDistances(minDistances, visited);
      int vertex = getVertexData[0];
      int currentMinDistance = getVertexData[1];

      if (currentMinDistance == Integer.MAX_VALUE) {
        break;
      }

      visited.add(vertex);

      for (int[] edge : edges[vertex]) {
        int destination = edge[0];
        int distanceToDestination = edge[1];

        if (visited.contains(destination)) {
          continue;
        }

        int newPathDistance = currentMinDistance + distanceToDestination;
        int currentDestinationDistance = minDistances[destination];
        if (newPathDistance < currentDestinationDistance) {
          minDistances[destination] = newPathDistance;
        }
      }
    }

    int[] finalDistances = new int[minDistances.length];
    for (int i = 0; i < minDistances.length; i++) {
      int distance = minDistances[i];
      if (distance == Integer.MAX_VALUE) {
        finalDistances[i] = -1;
      } else {
        finalDistances[i] = distance;
      }
    }

    return finalDistances;
  }

  public int[] getVertexWithMinDistances(int[] distances, Set<Integer> visited) {
    int currentMinDistance = Integer.MAX_VALUE;
    int vertex = -1;

    for (int vertexIdx = 0; vertexIdx < distances.length; vertexIdx++) {
      int distance = distances[vertexIdx];

      if (visited.contains(vertexIdx)) {
        continue;
      }

      if (distance <= currentMinDistance) {
        vertex = vertexIdx;
        currentMinDistance = distance;
      }
    }

    return new int[] {vertex, currentMinDistance};
  }
}
```

### Solution 2

```java
class Program {
  // O((v + e) * log(v)) time | O(v) space - where v is the number
  // of vertices and e is the number of edges in the input graph
  public int[] dijkstrasAlgorithm(int start, int[][][] edges) {
    int numberOfVertices = edges.length;

    int[] minDistances = new int[numberOfVertices];
    Arrays.fill(minDistances, Integer.MAX_VALUE);
    minDistances[start] = 0;

    List<Item> minDistancesPairs = new ArrayList<Item>();
    for (int i = 0; i < numberOfVertices; i++) {
      Item item = new Item(i, Integer.MAX_VALUE);
      minDistancesPairs.add(item);
    }

    MinHeap minDistancesHeap = new MinHeap(minDistancesPairs);
    minDistancesHeap.update(start, 0);

    while (!minDistancesHeap.isEmpty()) {
      Item heapItem = minDistancesHeap.remove();
      int vertex = heapItem.vertex;
      int currentMinDistance = heapItem.distance;

      if (currentMinDistance == Integer.MAX_VALUE) {
        break;
      }

      for (int[] edge : edges[vertex]) {
        int destination = edge[0];
        int distanceToDestination = edge[1];
        int newPathDistance = currentMinDistance + distanceToDestination;
        int currentDestinationDistance = minDistances[destination];
        if (newPathDistance < currentDestinationDistance) {
          minDistances[destination] = newPathDistance;
          minDistancesHeap.update(destination, newPathDistance);
        }
      }
    }

    int[] finalDistances = new int[minDistances.length];
    for (int i = 0; i < minDistances.length; i++) {
      int distance = minDistances[i];
      if (distance == Integer.MAX_VALUE) {
        finalDistances[i] = -1;
      } else {
        finalDistances[i] = distance;
      }
    }

    return finalDistances;
  }

  static class Item {
    int vertex;
    int distance;

    public Item(int vertex, int distance) {
      this.vertex = vertex;
      this.distance = distance;
    }
  };

  static class MinHeap {
    Map<Integer, Integer> vertexMap = new HashMap<Integer, Integer>();
    List<Item> heap = new ArrayList<Item>();

    public MinHeap(List<Item> array) {
      for (int i = 0; i < array.size(); i++) {
        Item item = array.get(i);
        vertexMap.put(item.vertex, item.vertex);
      }
      heap = buildHeap(array);
    }

    List<Item> buildHeap(List<Item> array) {
      int firstParentIdx = (array.size() - 2) / 2;
      for (int currentIdx = firstParentIdx + 1; currentIdx >= 0; currentIdx--) {
        siftDown(currentIdx, array.size() - 1, array);
      }
      return array;
    }

    boolean isEmpty() {
      return heap.size() == 0;
    }

    void siftDown(int currentIdx, int endIdx, List<Item> heap) {
      int childOneIdx = currentIdx * 2 + 1;
      while (childOneIdx <= endIdx) {
        int childTwoIdx = currentIdx * 2 + 2 <= endIdx ? currentIdx * 2 + 2 : -1;
        int idxToSwap;
        if (childTwoIdx != -1 && heap.get(childTwoIdx).distance < heap.get(childOneIdx).distance) {
          idxToSwap = childTwoIdx;
        } else {
          idxToSwap = childOneIdx;
        }
        if (heap.get(idxToSwap).distance < heap.get(currentIdx).distance) {
          swap(currentIdx, idxToSwap);
          currentIdx = idxToSwap;
          childOneIdx = currentIdx * 2 + 1;
        } else {
          return;
        }
      }
    }

    void siftUp(int currentIdx) {
      int parentIdx = (currentIdx - 1) / 2;
      while (currentIdx > 0 && heap.get(currentIdx).distance < heap.get(parentIdx).distance) {
        swap(currentIdx, parentIdx);
        currentIdx = parentIdx;
        parentIdx = (currentIdx - 1) / 2;
      }
    }

    Item remove() {
      if (isEmpty()) {
        return null;
      }

      swap(0, heap.size() - 1);
      Item lastItem = heap.get(heap.size() - 1);
      int vertex = lastItem.vertex;
      int distance = lastItem.distance;
      heap.remove(heap.size() - 1);
      vertexMap.remove(vertex);
      siftDown(0, heap.size() - 1, heap);
      return new Item(vertex, distance);
    }

    void update(int vertex, int value) {
      heap.set(vertexMap.get(vertex), new Item(vertex, value));
      siftUp(vertexMap.get(vertex));
    }

    void swap(int i, int j) {
      vertexMap.put(heap.get(i).vertex, j);
      vertexMap.put(heap.get(j).vertex, i);
      Item temp = heap.get(i);
      heap.set(i, heap.get(j));
      heap.set(j, temp);
    }
  }
}

```
