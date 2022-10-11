# AlgoExpert

## Tries 01 Suffix Trie Construction

### Question

Write a SuffixTrie class for a Suffix-Trie-like data structure. The class should have a root property set to be the root node of the trie and should support:

Creating the trie from a string; this will be done by calling the populateSuffixTrieFrom method upon class instantiation, which should populate the root of the class.
Searching for strings in the trie.
Note that every string added to the trie should end with the special endSymbol character: "*".

If you're unfamiliar with Suffix Tries, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

### Sample Input (for creation)

string = "babc"

### Sample Output (for creation)

The structure below is the root of the trie.
{
  "c": {"*": true},
  "b": {
    "c": {"*": true},
    "a": {"b": {"c": {"*": true}}},
  },
  "a": {"b": {"c": {"*": true}}},
}

### Sample Input (for searching in the suffix trie above)

string = "abc"

### Sample Output (for searching in the suffix trie above)

true

### Optimal Space & Time Complexity

Creation: O(n^2) time | O(n^2) space - where n is the length of the input string Searching: O(m) time | O(1) space - where m is the length of the input string

%

### Solution 1

```java
class Program {
  static class TrieNode {
    Map<Character, TrieNode> children = new HashMap<Character, TrieNode>();
  }

  static class SuffixTrie {
    TrieNode root = new TrieNode();
    char endSymbol = '*';

    public SuffixTrie(String str) {
      populateSuffixTrieFrom(str);
    }

    // O(n^2) time | O(n^2) space
    public void populateSuffixTrieFrom(String str) {
      for (int i = 0; i < str.length(); i++) {
        insertSubstringStartingAt(i, str);
      }
    }

    public void insertSubstringStartingAt(int i, String str) {
      TrieNode node = root;
      for (int j = i; j < str.length(); j++) {
        char letter = str.charAt(j);
        if (!node.children.containsKey(letter)) {
          TrieNode newNode = new TrieNode();
          node.children.put(letter, newNode);
        }
        node = node.children.get(letter);
      }
      node.children.put(endSymbol, null);
    }

    // O(m) time | O(1) space
    public boolean contains(String str) {
      TrieNode node = root;
      for (int i = 0; i < str.length(); i++) {
        char letter = str.charAt(i);
        if (!node.children.containsKey(letter)) {
          return false;
        }
        node = node.children.get(letter);
      }
      return node.children.containsKey(endSymbol);
    }
  }
}

```

### Solution 2

User solution

```java
class Program {
  // Do not edit the class below except for the
  // populateSuffixTrieFrom and contains methods.
  // Feel free to add new properties and methods
  // to the class.
  static class TrieNode {
    Map<Character, TrieNode> children = new HashMap<Character, TrieNode>();
  }

  static class SuffixTrie {
    TrieNode root = new TrieNode();
    char endSymbol = '*';

    public SuffixTrie(String str) {
      populateSuffixTrieFrom(str);
    }

    public void populateSuffixTrieFrom(String str) {
      
      

      for(int i = 0; i < str.length(); i++){
          String word = str.substring(i);
          TrieNode cur = root;
          for(Character c : word.toCharArray()){
            if(!cur.children.containsKey(c)){
              TrieNode tn = new TrieNode();
              cur.children.put(c, tn);
            }
            
            
            cur = cur.children.get(c);
          }
          cur.children.put(endSymbol, null);
      }
    }

    public boolean contains(String str) {
      TrieNode cur = root;
      for(Character c : str.toCharArray()){
        if(!cur.children.containsKey(c)){
          return false;
        }
        cur = cur.children.get(c);
      }
      if(cur.children.containsKey(endSymbol)) return true;
      return false;
    }
  }
}

```