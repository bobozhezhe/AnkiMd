# AlgoExpert

## Binary Search Trees 02 BST Construction

### Question

Write a BST class for a Binary Search Tree. The class should support:

Inserting values with the insert method.
Removing values with the remove method; this method should only remove the first instance of a given value.
Searching for values with the contains method.
Note that you can't remove values from a single-node tree. In other words, calling the remove method on a single-node tree should simply not do anything.

Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

### Sample Usage

// Assume the following BST has already been created:
         10
       /     \
      5      15
    /   \   /   \
   2     5 13   22
 /           \
1            14

// All operations below are performed sequentially.
insert(12):   10
            /     \
           5      15
         /   \   /   \
        2     5 13   22
      /        /  \
     1        12  14

remove(10):   12
            /     \
           5      15
         /   \   /   \
        2     5 13   22
      /           \
     1            14

contains(15): true

### Optimal Space & Time Complexity

Average (all 3 methods): O(log(n)) time | O(1) space - where n is the number of nodes in the BST Worst (all 3 methods): O(n) time | O(1) space - where n is the number of nodes in the BST

%

### Key Point

### Solution 1

```java
class Program {
  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }

    // Average: O(log(n)) time | O(log(n)) space
    // Worst: O(n) time | O(n) space
    public BST insert(int value) {
      if (value < this.value) {
        if (left == null) {
          BST newBST = new BST(value);
          left = newBST;
        } else {
          left.insert(value);
        }
      } else {
        if (right == null) {
          BST newBST = new BST(value);
          right = newBST;
        } else {
          right.insert(value);
        }
      }
      return this;
    }

    // Average: O(log(n)) time | O(log(n)) space
    // Worst: O(n) time | O(n) space
    public boolean contains(int value) {
      if (value < this.value) {
        if (left == null) {
          return false;
        } else {
          return left.contains(value);
        }
      } else if (value > this.value) {
        if (right == null) {
          return false;
        } else {
          return right.contains(value);
        }
      } else {
        return true;
      }
    }

    // Average: O(log(n)) time | O(log(n)) space
    // Worst: O(n) time | O(n) space
    public BST remove(int value) {
      remove(value, null);
      return this;
    }

    public void remove(int value, BST parent) {
      if (value < this.value) {
        if (left != null) {
          left.remove(value, this);
        }
      } else if (value > this.value) {
        if (right != null) {
          right.remove(value, this);
        }
      } else {
        if (left != null && right != null) {
          this.value = right.getMinValue();
          right.remove(this.value, this);
        } else if (parent == null) {
          if (left != null) {
            this.value = left.value;
            right = left.right;
            left = left.left;
          } else if (right != null) {
            this.value = right.value;
            left = right.left;
            right = right.right;
          } else {
            // This is a single-node tree; do nothing.
          }
        } else if (parent.left == this) {
          parent.left = left != null ? left : right;
        } else if (parent.right == this) {
          parent.right = left != null ? left : right;
        }
      }
    }

    public int getMinValue() {
      if (left == null) {
        return this.value;
      } else {
        return left.getMinValue();
      }
    }
  }
}

```

### Solution 2

```java
class Program {
  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }

    // Average: O(log(n)) time | O(1) space
    // Worst: O(n) time | O(1) space
    public BST insert(int value) {
      BST currentNode = this;
      while (true) {
        if (value < currentNode.value) {
          if (currentNode.left == null) {
            BST newNode = new BST(value);
            currentNode.left = newNode;
            break;
          } else {
            currentNode = currentNode.left;
          }
        } else {
          if (currentNode.right == null) {
            BST newNode = new BST(value);
            currentNode.right = newNode;
            break;
          } else {
            currentNode = currentNode.right;
          }
        }
      }
      return this;
    }

    // Average: O(log(n)) time | O(1) space
    // Worst: O(n) time | O(1) space
    public boolean contains(int value) {
      BST currentNode = this;
      while (currentNode != null) {
        if (value < currentNode.value) {
          currentNode = currentNode.left;
        } else if (value > currentNode.value) {
          currentNode = currentNode.right;
        } else {
          return true;
        }
      }
      return false;
    }

    // Average: O(log(n)) time | O(1) space
    // Worst: O(n) time | O(1) space
    public BST remove(int value) {
      remove(value, null);
      return this;
    }

    public void remove(int value, BST parentNode) {
      BST currentNode = this;
      while (currentNode != null) {
        if (value < currentNode.value) {
          parentNode = currentNode;
          currentNode = currentNode.left;
        } else if (value > currentNode.value) {
          parentNode = currentNode;
          currentNode = currentNode.right;
        } else {
          if (currentNode.left != null && currentNode.right != null) {
            currentNode.value = currentNode.right.getMinValue();
            currentNode.right.remove(currentNode.value, currentNode);
          } else if (parentNode == null) {
            if (currentNode.left != null) {
              currentNode.value = currentNode.left.value;
              currentNode.right = currentNode.left.right;
              currentNode.left = currentNode.left.left;
            } else if (currentNode.right != null) {
              currentNode.value = currentNode.right.value;
              currentNode.left = currentNode.right.left;
              currentNode.right = currentNode.right.right;
            } else {
              // This is a single-node tree; do nothing.
            }
          } else if (parentNode.left == currentNode) {
            parentNode.left = currentNode.left != null ? currentNode.left : currentNode.right;
          } else if (parentNode.right == currentNode) {
            parentNode.right = currentNode.left != null ? currentNode.left : currentNode.right;
          }
          break;
        }
      }
    }

    public int getMinValue() {
      if (left == null) {
        return value;
      } else {
        return left.getMinValue();
      }
    }
  }
}

```
