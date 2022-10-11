# AlgoExpert

## Stack 06 Shorten Path

### Question

Write a function that takes in a non-empty string representing a valid Unix-shell path and returns a shortened version of that path.

A path is a notation that represents the location of a file or directory in a file system.

A path can be an absolute path, meaning that it starts at the root directory in a file system, or a relative path, meaning that it starts at the current directory in a file system.

In a Unix-like operating system, a path is bound by the following rules:

The root directory is represented by a /. This means that if a path starts with /, it's an absolute path; if it doesn't, it's a relative path.
The symbol / otherwise represents the directory separator. This means that the path /foo/bar is the location of the directory bar inside the directory foo, which is itself located inside the root directory.
The symbol .. represents the parent directory. This means that accessing files or directories in /foo/bar/.. is equivalent to accessing files or directories in /foo.
The symbol . represents the current directory. This means that accessing files or directories in /foo/bar/. is equivalent to accessing files or directories in /foo/bar.
The symbols / and . can be repeated sequentially without consequence; the symbol .. cannot, however, because repeating it sequentially means going further up in parent directories. For example, /foo/bar/baz/././. and /foo/bar/baz are equivalent paths, but /foo/bar/baz/../../../ and /foo/bar/baz definitely aren't. The only exception is with the root directory: /../../.. and / are equivalent, because the root directory has no parent directory, which means that repeatedly accessing parent directories does nothing.
Note that the shortened version of the path must be equivalent to the original path. In other words, it must point to the same file or directory as the original path.

### Sample Input

path = "/foo/../test/../test/../foo//bar/./baz"

### Sample Output

"/foo/bar/baz" // This path is equivalent to the input path.

### Optimal Space & Time Complexity

O(n) time | O(n) space - where n is the length of the pathname

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(n) space - where n is the length of the path
  public static String shortenPath(String path) {
    boolean startsWithPath = path.charAt(0) == '/';
    String[] tokensArr = path.split("/");
    List<String> tokensList = Arrays.asList(tokensArr);
    List<String> filteredTokens =
        tokensList.stream().filter(token -> isImportantToken(token)).collect(Collectors.toList());
    List<String> stack = new ArrayList<String>();
    if (startsWithPath) stack.add("");
    for (String token : filteredTokens) {
      if (token.equals("..")) {
        if (stack.size() == 0 || stack.get(stack.size() - 1).equals("..")) {
          stack.add(token);
        } else if (!stack.get(stack.size() - 1).equals("")) {
          stack.remove(stack.size() - 1);
        }
      } else {
        stack.add(token);
      }
    }

    if (stack.size() == 1 && stack.get(0).equals("")) return "/";
    return String.join("/", stack);
  }

  public static boolean isImportantToken(String token) {
    return token.length() > 0 && !token.equals(".");
  }
}

```
