# AlgoExpert

## Graphs 05 Youngest Common Ancestor

### Question

You're given three inputs, all of which are instances of an AncestralTree class that have an ancestor property pointing to their youngest ancestor. The first input is the top ancestor in an ancestral tree (i.e., the only instance that has no ancestor--its ancestor property points to None / null), and the other two inputs are descendants in the ancestral tree.

Write a function that returns the youngest common ancestor to the two descendants.

Note that a descendant is considered its own ancestor. So in the simple ancestral tree below, the youngest common ancestor to nodes A and B is node A.

// The youngest common ancestor to nodes A and B is node A.
  A
 /
B

### Sample Input

// The nodes are from the ancestral tree below.
topAncestor = node A
descendantOne = node E
descendantTwo = node I
          A
       /     \
      B       C
    /   \   /   \
   D     E F     G
 /   \
H     I

### Sample Output

node B

### Optimal Space & Time Complexity

O(d) time | O(1) space - where d is the depth (height) of the ancestral tree

%

### Key Point

### Solution 1

```java
class Program {
  // O(d) time | O(1) space - where d is the depth (height) of the ancestral tree
  public static AncestralTree getYoungestCommonAncestor(
      AncestralTree topAncestor, AncestralTree descendantOne, AncestralTree descendantTwo) {
    int depthOne = getDescendantDepth(descendantOne, topAncestor);
    int depthTwo = getDescendantDepth(descendantTwo, topAncestor);
    if (depthOne > depthTwo) {
      return backtrackAncestralTree(descendantOne, descendantTwo, depthOne - depthTwo);
    } else {
      return backtrackAncestralTree(descendantTwo, descendantOne, depthTwo - depthOne);
    }
  }

  public static int getDescendantDepth(AncestralTree descendant, AncestralTree topAncestor) {
    int depth = 0;
    while (descendant != topAncestor) {
      depth++;
      descendant = descendant.ancestor;
    }
    return depth;
  }

  public static AncestralTree backtrackAncestralTree(
      AncestralTree lowerDescendant, AncestralTree higherDescendant, int diff) {
    while (diff > 0) {
      lowerDescendant = lowerDescendant.ancestor;
      diff--;
    }
    while (lowerDescendant != higherDescendant) {
      lowerDescendant = lowerDescendant.ancestor;
      higherDescendant = higherDescendant.ancestor;
    }
    return lowerDescendant;
  }

  static class AncestralTree {
    public char name;
    public AncestralTree ancestor;

    AncestralTree(char name) {
      this.name = name;
      this.ancestor = null;
    }

    // This method is for testing only.
    void addAsAncestor(AncestralTree[] descendants) {
      for (AncestralTree descendant : descendants) {
        descendant.ancestor = this;
      }
    }
  }
}

```
