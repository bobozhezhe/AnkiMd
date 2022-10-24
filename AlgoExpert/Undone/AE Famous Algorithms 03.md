# AlgoExpert

## Famous Algorithms 03 Topological Sort

### Question

You're given a list of arbitrary jobs that need to be completed; these jobs are represented by distinct integers. You're also given a list of dependencies. A dependency is represented as a pair of jobs where the first job is a prerequisite of the second one. In other words, the second job depends on the first one; it can only be completed once the first job is completed.

Write a function that takes in a list of jobs and a list of dependencies and returns a list containing a valid order in which the given jobs can be completed. If no such order exists, the function should return an empty array.

### Sample Input

jobs = [1, 2, 3, 4]
deps = [[1, 2], [1, 3], [3, 2], [4, 2], [4, 3]]

### Sample Output

[1, 4, 3, 2] or [4, 1, 3, 2]

### Optimal Space & Time Complexity

O(j + d) time | O(j + d) space - where j is the number of jobs and d is the number of dependencies

%

### Key Point

### Solution 1

```java
class Program {
  // O(j + d) time | O(j + d) space
  public static List<Integer> topologicalSort(List<Integer> jobs, List<Integer[]> deps) {
    JobGraph jobGraph = createJobGraph(jobs, deps);
    return getOrderedJobs(jobGraph);
  }

  public static JobGraph createJobGraph(List<Integer> jobs, List<Integer[]> deps) {
    JobGraph graph = new JobGraph(jobs);
    for (Integer[] dep : deps) {
      graph.addPrereq(dep[1], dep[0]);
    }
    return graph;
  }

  public static List<Integer> getOrderedJobs(JobGraph graph) {
    List<Integer> orderedJobs = new ArrayList<Integer>();
    List<JobNode> nodes = new ArrayList<JobNode>(graph.nodes);
    while (nodes.size() > 0) {
      JobNode node = nodes.get(nodes.size() - 1);
      nodes.remove(nodes.size() - 1);
      boolean containsCycle = depthFirstTraverse(node, orderedJobs);
      if (containsCycle) return new ArrayList<Integer>();
    }
    return orderedJobs;
  }

  public static boolean depthFirstTraverse(JobNode node, List<Integer> orderedJobs) {
    if (node.visited) return false;
    if (node.visiting) return true;
    node.visiting = true;
    for (JobNode prereqNode : node.prereqs) {
      boolean containsCycle = depthFirstTraverse(prereqNode, orderedJobs);
      if (containsCycle) return true;
    }
    node.visited = true;
    node.visiting = false;
    orderedJobs.add(node.job);
    return false;
  }

  static class JobGraph {
    public List<JobNode> nodes;
    public Map<Integer, JobNode> graph;

    public JobGraph(List<Integer> jobs) {
      nodes = new ArrayList<JobNode>();
      graph = new HashMap<Integer, JobNode>();
      for (Integer job : jobs) {
        addNode(job);
      }
    }

    public void addPrereq(Integer job, Integer prereq) {
      JobNode jobNode = getNode(job);
      JobNode prereqNode = getNode(prereq);
      jobNode.prereqs.add(prereqNode);
    }

    public void addNode(Integer job) {
      graph.put(job, new JobNode(job));
      nodes.add(graph.get(job));
    }

    public JobNode getNode(Integer job) {
      if (!graph.containsKey(job)) addNode(job);
      return graph.get(job);
    }
  }

  static class JobNode {
    public Integer job;
    public List<JobNode> prereqs;
    public boolean visited;
    public boolean visiting;

    public JobNode(Integer job) {
      this.job = job;
      prereqs = new ArrayList<JobNode>();
      visited = false;
      visiting = false;
    }
  }
}
```

### Solution 2

```java
class Program {
  // O(j + d) time | O(j + d) space
  public static List<Integer> topologicalSort(List<Integer> jobs, List<Integer[]> deps) {
    JobGraph jobGraph = createJobGraph(jobs, deps);
    return getOrderedJobs(jobGraph);
  }

  public static JobGraph createJobGraph(List<Integer> jobs, List<Integer[]> deps) {
    JobGraph graph = new JobGraph(jobs);
    for (Integer[] dep : deps) {
      graph.addDep(dep[0], dep[1]);
    }
    return graph;
  }

  public static List<Integer> getOrderedJobs(JobGraph graph) {
    List<Integer> orderedJobs = new ArrayList<Integer>();
    List<JobNode> nodesWithNoPrereqs = new ArrayList<JobNode>();
    for (JobNode node : graph.nodes) {
      if (node.numOfPrereqs == 0) {
        nodesWithNoPrereqs.add(node);
      }
    }
    while (nodesWithNoPrereqs.size() > 0) {
      JobNode node = nodesWithNoPrereqs.get(nodesWithNoPrereqs.size() - 1);
      nodesWithNoPrereqs.remove(nodesWithNoPrereqs.size() - 1);
      orderedJobs.add(node.job);
      removeDeps(node, nodesWithNoPrereqs);
    }
    boolean graphHasEdges = false;
    for (JobNode node : graph.nodes) {
      if (node.numOfPrereqs > 0) {
        graphHasEdges = true;
      }
    }
    return graphHasEdges ? new ArrayList<Integer>() : orderedJobs;
  }

  public static void removeDeps(JobNode node, List<JobNode> nodesWithNoPrereqs) {
    while (node.deps.size() > 0) {
      JobNode dep = node.deps.get(node.deps.size() - 1);
      node.deps.remove(node.deps.size() - 1);
      dep.numOfPrereqs--;
      if (dep.numOfPrereqs == 0) nodesWithNoPrereqs.add(dep);
    }
  }

  static class JobGraph {
    public List<JobNode> nodes;
    public Map<Integer, JobNode> graph;

    public JobGraph(List<Integer> jobs) {
      nodes = new ArrayList<JobNode>();
      graph = new HashMap<Integer, JobNode>();
      for (Integer job : jobs) {
        addNode(job);
      }
    }

    public void addDep(Integer job, Integer dep) {
      JobNode jobNode = getNode(job);
      JobNode depNode = getNode(dep);
      jobNode.deps.add(depNode);
      depNode.numOfPrereqs++;
    }

    public void addNode(Integer job) {
      graph.put(job, new JobNode(job));
      nodes.add(graph.get(job));
    }

    public JobNode getNode(Integer job) {
      if (!graph.containsKey(job)) addNode(job);
      return graph.get(job);
    }
  }

  static class JobNode {
    public Integer job;
    public List<JobNode> deps;
    public Integer numOfPrereqs;

    public JobNode(Integer job) {
      this.job = job;
      deps = new ArrayList<JobNode>();
      numOfPrereqs = 0;
    }
  }
}
```
