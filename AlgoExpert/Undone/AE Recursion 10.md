# AlgoExpert

## Recursion 10 Generate Div Tags

### Question

Write a function that takes in a positive integer numberOfTags and returns a list of all the valid strings that you can generate with that number of matched `<div></div>` tags.

A string is valid and contains matched `<div></div>` tags if for every opening tag `<div>`, there's a closing tag `</div>` that comes after the opening tag and that isn't used as a closing tag for another opening tag. Each output string should contain exactly numberOfTags opening tags and numberOfTags closing tags.

For example, given numberOfTags = 2, the valid strings to return would be: `["<div></div><div></div>", "<div><div></div></div>"]`.

Note that the output strings don't need to be in any particular order.

### Sample Input

numberOfTags = 3

### Sample Output

```html
  [
    "<div><div><div></div></div></div>",
    "<div><div></div><div></div></div>",
    "<div><div></div></div><div></div>",
    "<div></div><div><div></div></div>",
    "<div></div><div></div><div></div>",
  ] // The strings could be ordered differently.
```

### Optimal Space & Time Complexity

O((2n)!/((n!((n + 1)!)))) time | O((2n)!/((n!((n + 1)!)))) space - where n is the input number

%

### Key Point

### Solution 1

```java
class Program {
  // O((2n)!/((n!((n + 1)!)))) time | O((2n)!/((n!((n + 1)!)))) space -
  // where n is the input number
  public ArrayList<String> generateDivTags(int numberOfTags) {
    ArrayList<String> matchedDivTags = new ArrayList<String>();
    generateDivTagsFromPrefix(numberOfTags, numberOfTags, "", matchedDivTags);
    return matchedDivTags;
  }

  public void generateDivTagsFromPrefix(
      int openingTagsNeeded, int closingTagsNeeded, String prefix, ArrayList<String> result) {
    if (openingTagsNeeded > 0) {
      String newPrefix = prefix + "<div>";
      generateDivTagsFromPrefix(openingTagsNeeded - 1, closingTagsNeeded, newPrefix, result);
    }

    if (openingTagsNeeded < closingTagsNeeded) {
      String newPrefix = prefix + "</div>";
      generateDivTagsFromPrefix(openingTagsNeeded, closingTagsNeeded - 1, newPrefix, result);
    }

    if (closingTagsNeeded == 0) {
      result.add(prefix);
    }
  }
}

```
