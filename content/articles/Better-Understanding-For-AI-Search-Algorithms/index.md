+++
title = 'Better Understanding for AI Search Algorithms'
date = 2024-02-02T23:53:51+02:00
description = "A brief description about AI Search Algorithms"
slug = "better-understanding-for-ai-search-algorithms"
authors = ["Mohamed Adel"]
math = true
tags = [
    "AI",
    "Search Algorithms",
    "Algorithms",
    "Python"
]
categories = ["Artificial Intelligence"]
+++

# Table of Contents
* [Main Idea](#main-idea)
* [Types of AI Search Algorithms](#types-of-ai-search-algorithms)
* [Uninformed Search Algorithms](#uninformed-search-algorithms)
  * [Depth-First Search (DFS)](#depth-first-search-dfs)
  * [Breadth-First Search (BFS)](#breadth-first-search-bfs)
  * [Depth-Limited Search (DLS)](#depth-limited-search-dls)
  * [Iterative Deepening DFS (ID-DFS)](#iterative-deepening-dfs-id-dfs)
  * [Uniform Cost Search (UCS)](#uniform-cost-search-ucs)

---

## Main Idea
Let us imagine that Engineers do not yet implement these kinds of algorithms,
and we have 
to think about modeling a life problem into a graph that represents all the states that this problem could be in.
For sure, the solution to our problem is a state in this graph.

Let us also think about the different cases of our knowledge about these states.

* **Case one:** that we already know the solution to our problem, and we just need to find a way to achieve this solution.

* **Case two:** that there could be multiple solutions to our problem that we don't know, but we have the ability to judge if the solution is correct or not.

{{< notice note >}}
This is just a way of thinking about search techniques, not some theoretical perspective of AI search.
{{< /notice >}}

---

## Types of AI Search Algorithms
From this way of thinking, we could divide our search techniques into two main categories:
* **Uninformed and Informed Search**
* **Constraints Satisfaction Problems (CSPs)**

In uninformed and informed searches, **we know the goal** we want to reach,
and we are looking for a **path** to achieve this goal from our current state.
In fact, we may not just look for any path.
Why not consider looking for the best path?
As an example, in 8-puzzle problem we know our goal state should be like in **[Figure 1]**.
We just need to find the path to this state from any given initial state.

{{<figure "photo" "fig1.svg" "Figure 1:" "Goal state in 8-puzzle problem">}}

In CSPs, we know some constraints that we can judge if the state we have reached is a valid goal state or not.
As an example, imagine you have a list of numbers that could be put next to each other at any order.
And then you have all possible arrangements that these numbers could be in.
But there are some constraints on these numbers like, for example,
that any 5 should be next to 4 and any 9 should not be next to 10.
Then you should look at these possibilities one by one, and on each possible arrangement of these numbers
you should check if it violates the constraints or not.
If it does not violate any constraints, then this could be one of the solutions.
Note that there could be many possibilities
that do not violate the constraints, therefore, there could be many solutions to this problem.

Now we have a better understanding of how these algorithms should work. Let us summarize these words at several points:
* **State space:** the list of states that the problem could be in
* **Successor function:** the successor states of the current state
* **Initial state:** the state that I am initially in
* **Goal state:** the desired state
* **path:** the path from the initial state to the goal state

In every algorithm, we will consider these points:
* **Completeness:** is it guaranteed that the algorithm will find the solution?
* **Optimality:** will the algorithm find the optimal solution?
* **Time complexity:** how many nodes did the algorithm have to expand?
* **Space complexity:** required space for the algorithm to find the solution?

In this story, we will focus on uninformed search algorithms.

---

## Uninformed Search Algorithms
We have several algorithms under this search category. Every algorithm has its own pros and cons that we will discuss.
* Depth-First Search (DFS)
* Breadth-First Search (BFS)
* Depth-Limited Search (DLS)
* Iterative Deepening DFS (ID-DFS)
* Uniform Cost Search (UCS)

What do we need to implement these algorithms?
* **Frontier (aka Fringe):** we use this data structure to keep track of what to explore next. We may use Stack, Queue, or Priority Queue
* **Closed Set:** we use this data structure to keep track of what we already explored to avoid repetitive work
* **Parents Directory:** to keep track of every state's parent. We will use this directory to extract the path

These Algorithms differ only in the successor function, which is the states to be explored next.
We use different data structures usually called fringe or frontier to control these exploration techniques.

---

### Depth-First Search (DFS)
#### Overview
In DFS, we explore nodes with the highest depth first like in **[Figure 2]**.

{{<figure "video" "fig2.mp4" "Figure 2:" "DFS Visualization">}}

#### Frontier
We choose the Stack data structure to be our frontier in DFS.

**The main idea of using Stack is:** the state pushes its successor states in the stack
and takes the top of the Stack and pushes its successor states and so on until we reach a leaf node,
then we start to backtrack.
This ensures that we will explore nodes with the highest depth first.

{{< notice note >}}
Why we need both (Frontier, Closed Set).
If the state is on the frontier, that does not mean that it has to be in the Closed Set and vice versa.
If the state is on the frontier, that means that this state will be explored soon.
If the state is in the Closed Set that means, it has already been explored, and we pushed all its successor states.
{{< /notice >}}

#### Implementation
```python
def dfs(initial_state):
    frontier = [initial_state]
    explored = set()
    parents = dict(initial_state: initial_state)

    while len(frontier) > 0:
        state = frontier.pop()
        explored.add(state)

        if state == goal_state:
            return parents

        neighbors = get_neighbors(state)
        for neighbor in neighbors:
            if neighbor not in parents and neighbor not in explored:
                frontier.append(neighbor)
                parents[neighbor] = state

    return None
```

First, we initialize the frontier to contain only the initial state.
And the explored set to be empty.
And the parents' directory contains the initial state as a parent to itself
(This will help to know that this is the root of the tree).

Second, we start looping as long the frontier is not empty.
If there is no solution,
then we will explore the whole search tree until the frontier is empty without finding any solution.
In this case, we return None meaning that there is no solution.

Third,
if there is a solution, then we start popping the current state from the frontier and add it to the explored set
(to avoid repetitive work).
If it is the goal state, then we return the parents' directory.
If not,
then we get its neighbors
and push them one by one into the frontier
after checking that this neighbor state is neither in the frontier nor in the explored set.

{{< notice note >}}
We check if the state is a goal state when we are popping it from the frontier.
Not when we are pushing it into the frontier.
{{< /notice >}}

#### Completeness
DFS is not complete because it may go deep into an infinite branch.

#### Optimality
DFS is not optimal because it finds the most left solution branch.
There could be better branches that lead to the solution than this branch.

#### Time Complexity
If our search tree has a branching factor (b) and our solution is at depth (m).
The worst-case scenario will be that the goal state is the rightest leaf node in the search tree.
We will search the whole tree.
Then the time complexity will be the number of nodes in our search tree.

Number of expanded nodes = $1 + b + b^2 + b^3 + ... + b^m$

Time complexity = Number of expanded nodes = $O(b^m)$

#### Space Complexity
In our frontier, we will store the successors
presented in the branch from the initial state to the goal state like in **[Figure 3]**.

We will store (b) nodes for (m) depth.

Space complexity = $O(b*m)$

{{<figure "photo" "fig3.svg" "Figure 3:" "Visualization of the nodes pushed into the frontier in DFS">}}

---

### Breadth-First Search (BFS)
#### Overview
In BFS, we explore nodes with the shallowest depth first like in **[Figure 4]**.

{{<figure "video" "fig4.mp4" "Figure 4:" "BFS Visualization">}}

#### Frontier
We choose the Queue data structure to be our frontier in BFS.

**Main idea of using Queue is:** 
the state pushes its successor states into the Queue
and takes the top of the Queue and pushes its successor states into 
the Queue and so on until we reach a leaf node, then we start to backtrack.

#### Implementation
```python
def bfs(initial_state):
    frontier = [initial_state]
    explored = set()
    parents = dict(initial_state: initial_state)

    while len(frontier) > 0:
        state = frontier.pop(0)
        explored.add(state)
    
        if state == goal_state:
            return parents

        neighbors = get_neighbors(state)
        for neighbor in neighbors:
            if neighbor not in parents and neighbor not in explored:
                frontier.append(neighbor)
                parents[neighbor] = state

    return None
```

Same steps in DFS, but with a Queue data structure as our frontier.

#### Completeness
BFS is complete because it searches the tree level by level. So, it will not go in an infinite branch.

#### Optimality
BFS is optimal only if all branches have the same cost, because it finds the path with the least number of edges,
therefore, it will find the optimal path if all branches have the same cost.

#### Time Complexity
If our search tree has a branching factor (b) and our solution is at depth (m).
The worst-case scenario will be that the goal state is the rightest leaf node in the search tree.
We will search the whole tree.
Then the time complexity will be the number of nodes in our search tree.

Number of expanded nodes = $1 + b + b^2 + b^3 + ... + b^m$

Time complexity = Number of expanded nodes = $O(b^m)$

#### Space Complexity
In our frontier, we will store at least all nodes in a level when exploring this level.
The worst-case scenario that our goal is a leaf node, so our frontier will contain all leaves like in **[Figure 5]**.

The number of leaves = $b^m$

Space complexity = $O(b^m)$

{{<figure "photo" "fig5.svg" "Figure 5:" "Visualization of the nodes pushed into the frontier in BFS">}}

---

### Depth-Limited Search (DLS)
#### Overview
In DLS, we explore nodes with the highest depth first like in **[Figure 6]**.
But the difference here that we explore until we reach a specific depth.
    
{{<figure "gif" "fig6.gif" "Figure 6:" "DLS Visualization">}}

#### Frontier
We choose the Stack data structure to be our frontier in DLS like in DFS.

#### Implementation
```python
def dls(initial_state, max_depth):
    frontier = [initial_state]
    explored = set()
    parents = dict(initial_state: initial_state)
    initial_state.depth = 0

    while len(frontier) > 0:
        state = frontier.pop()
        explored.add(state)

        if state.depth == max_depth:
            continue

        if state == goal_state:
            return parents

        neighbors = get_neighbors(state)
        for neighbor in neighbors:
            if neighbor not in parents and neighbor not in explored:
                frontier.append(neighbor)
                neighbor.depth = state.depth + 1
                parents[neighbor] = state

    return None
```

Same steps in DFS,
but we check if the depth reaches the specified max depth then we continue with other nodes in the fringe.

#### Completeness
DLS is not complete because it backtracks if it reaches a specified depth.
The goal state may lie in the next level like in **[Figure 6]**.

#### Optimality
The DLS is not optimal for the same reasons in DFS.

#### Time Complexity
Same as DFS, but with depth (d) (the maximum depth).

Time complexity = $O(b^d)$

#### Space Complexity
Same as DFS, but with depth (d) (the maximum depth).

Space complexity = $O(b*d)$

---

### Iterative Deepening DFS (ID-DFS)
#### Overview
In ID-DFS we do DLS for max depth from 1 to m, where (m) is the depth at which the goal node lies.

{{<figure "gif" "fig7.gif" "Figure 7:" "ID-DFS Visualization">}}

#### Frontier
We choose the Stack data structure to be our frontier in ID-DLS like in DFS and DLS.

#### Implementation
```python
def dls(initial_state, max_depth):
    frontier = [initial_state]
    explored = set()
    parents = dict(initial_state: initial_state)
    initial_state.depth = 0

    while len(frontier) > 0:
        state = frontier.pop()
        explored.add(state)

        if state.depth == max_depth:
            continue

        if state == goal_state:
            return parents

        neighbors = get_neighbors(state)
        for neighbor in neighbors:
            if neighbor not in parents and neighbor not in explored:
                frontier.append(neighbor)
                neighbor.depth = state.depth + 1
                parents[neighbor] = state

    return None
```

```python
def iddfs(initial_state):
    state = None
    d = 1

    while state is None:
          state = dls(initial_state, d)
          d += 1

    return state
```

We run DLS for d initially equals one until we find our goal state.

#### Completeness
ID-DLS is complete, it runs level by level until finding the goal state. In other words, it uses DFS and BFS together.

#### Optimality
ID-DLS is optimal, it runs level by level until finding the goal state,
so it will find the path with the least number of edges.
But like BFS, it is optimal only if all branches have the same cost.

#### Time Complexity
The goal node lies at depth (m).
So, we will run DLS for (m) times.
Each time we will run DLS for a specific depth.
So, the time complexity will be the sum of the time complexity of DLS for each depth.

Time complexity = $O(b + b^2 + b^3 + ... + b^m)$ = $O(b^m)$

#### Space Complexity
The space complexity will be the space complexity of DLS for the maximum depth.

Space complexity = $O(b*m)$

---

### Uniform Cost Search (UCS)
#### Overview
In UCS, we explore nodes with the least cost first.

#### Frontier
We choose the Priority Queue data structure to be our frontier in UCS.

**Main idea of using Priority Queue is:**
the state pushes its successor states into the Priority Queue
and takes the top of the Priority Queue and pushes its successor states into the Priority Queue
and so on until we reach a leaf node, then we start to backtrack.

#### Implementation
```python
def ucs(initial_state):
    frontier = [(0, initial_state)]
    explored = set()
    parents = dict(initial_state: initial_state)

    while len(frontier) > 0:
        state = frontier.pop(0)
        explored.add(state)

        if state == goal_state:
            return parents

        neighbors = get_neighbors(state)
        for neighbor in neighbors:
            if neighbor not in parents and neighbor not in explored:
                frontier.append((neighbor.cost, neighbor))
                frontier.sort(key=lambda x: x[0])
                parents[neighbor] = state

    return None
```

Same steps in BFS, but with a Priority Queue data structure as our frontier.

#### Completeness
UCS is complete because it searches the tree level by level. So, it will not go in an infinite branch.

#### Optimality
UCS is optimal because it finds the path with the least cost.

#### Time Complexity
Same as BFS, but with the cost of each edge.

Time complexity = $O(b^m)$

#### Space Complexity
Same as BFS, but with the cost of each edge.

Space complexity = $O(b^m)$