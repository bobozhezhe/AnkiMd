# AlgoExpert

## Recursion 08 Lowest Common Manager

### Question

You're given three inputs, all of which are instances of an OrgChart class that have a directReports property pointing to their direct reports. The first input is the top manager in an organizational chart (i.e., the only instance that isn't anybody else's direct report), and the other two inputs are reports in the organizational chart. The two reports are guaranteed to be distinct.

Write a function that returns the lowest common manager to the two reports.

### Sample Input

// From the organizational chart below.
topManager = Node A
reportOne = Node E
reportTwo = Node I
          A
       /     \
      B       C
    /   \   /   \
   D     E F     G
 /   \
H     I

### Sample Output

Node B

### Optimal Space & Time Complexity

O(n) time | O(d) space - where n is the number of people in the org and d is the depth (height) of the org chart

%

### Key Point

### Solution 1

```java
class Program {
  // O(n) time | O(d) space - where n is the number of people
  // in the org and d is the depth (height) of the org chart
  public static OrgChart getLowestCommonManager(
      OrgChart topManager, OrgChart reportOne, OrgChart reportTwo) {
    return getOrgInfo(topManager, reportOne, reportTwo).lowestCommonManager;
  }

  public static OrgInfo getOrgInfo(OrgChart manager, OrgChart reportOne, OrgChart reportTwo) {
    int numImportantReports = 0;
    for (OrgChart directReport : manager.directReports) {
      OrgInfo orgInfo = getOrgInfo(directReport, reportOne, reportTwo);
      if (orgInfo.lowestCommonManager != null) return orgInfo;
      numImportantReports += orgInfo.numImportantReports;
    }
    if (manager == reportOne || manager == reportTwo) numImportantReports++;
    OrgChart lowestCommonManager = numImportantReports == 2 ? manager : null;
    OrgInfo newOrgInfo = new OrgInfo(lowestCommonManager, numImportantReports);
    return newOrgInfo;
  }

  static class OrgChart {
    public char name;
    public List<OrgChart> directReports;

    OrgChart(char name) {
      this.name = name;
      this.directReports = new ArrayList<OrgChart>();
    }

    // This method is for testing only.
    public void addDirectReports(OrgChart[] directReports) {
      for (OrgChart directReport : directReports) {
        this.directReports.add(directReport);
      }
    }
  }

  static class OrgInfo {
    public OrgChart lowestCommonManager;
    int numImportantReports;

    OrgInfo(OrgChart lowestCommonManager, int numImportantReports) {
      this.lowestCommonManager = lowestCommonManager;
      this.numImportantReports = numImportantReports;
    }
  }
}

```
