# AlgoExpert

## Graphs 09 Boggle Board

### Question

You're given a two-dimensional array (a matrix) of potentially unequal height and width containing letters; this matrix represents a boggle board. You're also given a list of words.

Write a function that returns an array of all the words contained in the boggle board. The final words don't need to be in any particular order.

A word is constructed in the boggle board by connecting adjacent (horizontally, vertically, or diagonally) letters, without using any single letter at a given position more than once; while a word can of course have repeated letters, those repeated letters must come from different positions in the boggle board in order for the word to be contained in the board. Note that two or more words are allowed to overlap and use the same letters in the boggle board.

### Sample Input

board = [
  ["t", "h", "i", "s", "i", "s", "a"],
  ["s", "i", "m", "p", "l", "e", "x"],
  ["b", "x", "x", "x", "x", "e", "b"],
  ["x", "o", "g", "g", "l", "x", "o"],
  ["x", "x", "x", "D", "T", "r", "a"],
  ["R", "E", "P", "E", "A", "d", "x"],
  ["x", "x", "x", "x", "x", "x", "x"],
  ["N", "O", "T", "R", "E", "-", "P"],
  ["x", "x", "D", "E", "T", "A", "E"],
],
words = [
  "this", "is", "not", "a", "simple", "boggle",
  "board", "test", "REPEATED", "NOTRE-PEATED",
]

### Sample Output

["this", "is", "a", "simple", "boggle", "board", "NOTRE-PEATED"]
// The words could be ordered differently.

### Optimal Space & Time Complexity

O(nm*8^s + ws) time | O(nm + ws) space - where n is the width the board, m is the height of the board, w is the number of words, and s is the length of the longest word

%

### Key Point

### Solution 1

```java
class Program {
  // O(nm*8^s + ws) time | O(nm + ws) space
  public static List<String> boggleBoard(char[][] board, String[] words) {
    Trie trie = new Trie();
    for (String word : words) {
      trie.add(word);
    }
    Set<String> finalWords = new HashSet<String>();
    boolean[][] visited = new boolean[board.length][board[0].length];
    for (int i = 0; i < board.length; i++) {
      for (int j = 0; j < board[0].length; j++) {
        explore(i, j, board, trie.root, visited, finalWords);
      }
    }
    List<String> finalWordsArray = new ArrayList<String>();
    finalWordsArray.addAll(finalWords);
    return finalWordsArray;
  }

  public static void explore(
      int i,
      int j,
      char[][] board,
      TrieNode trieNode,
      boolean[][] visited,
      Set<String> finalWords) {
    if (visited[i][j]) {
      return;
    }
    char letter = board[i][j];
    if (!trieNode.children.containsKey(letter)) {
      return;
    }
    visited[i][j] = true;
    trieNode = trieNode.children.get(letter);
    if (trieNode.children.containsKey('*')) {
      finalWords.add(trieNode.word);
    }
    List<Integer[]> neighbors = getNeighbors(i, j, board);
    for (Integer[] neighbor : neighbors) {
      explore(neighbor[0], neighbor[1], board, trieNode, visited, finalWords);
    }
    visited[i][j] = false;
  }

  public static List<Integer[]> getNeighbors(int i, int j, char[][] board) {
    List<Integer[]> neighbors = new ArrayList<Integer[]>();
    if (i > 0 && j > 0) {
      neighbors.add(new Integer[] {i - 1, j - 1});
    }
    if (i > 0 && j < board[0].length - 1) {
      neighbors.add(new Integer[] {i - 1, j + 1});
    }
    if (i < board.length - 1 && j < board[0].length - 1) {
      neighbors.add(new Integer[] {i + 1, j + 1});
    }
    if (i < board.length - 1 && j > 0) {
      neighbors.add(new Integer[] {i + 1, j - 1});
    }
    if (i > 0) {
      neighbors.add(new Integer[] {i - 1, j});
    }
    if (i < board.length - 1) {
      neighbors.add(new Integer[] {i + 1, j});
    }
    if (j > 0) {
      neighbors.add(new Integer[] {i, j - 1});
    }
    if (j < board[0].length - 1) {
      neighbors.add(new Integer[] {i, j + 1});
    }
    return neighbors;
  }

  static class TrieNode {
    Map<Character, TrieNode> children = new HashMap<Character, TrieNode>();
    String word = "";
  }

  static class Trie {
    TrieNode root;
    char endSymbol;

    public Trie() {
      this.root = new TrieNode();
      this.endSymbol = '*';
    }

    public void add(String str) {
      TrieNode node = this.root;
      for (int i = 0; i < str.length(); i++) {
        char letter = str.charAt(i);
        if (!node.children.containsKey(letter)) {
          TrieNode newNode = new TrieNode();
          node.children.put(letter, newNode);
        }
        node = node.children.get(letter);
      }
      node.children.put(this.endSymbol, null);
      node.word = str;
    }
  }
}

```
