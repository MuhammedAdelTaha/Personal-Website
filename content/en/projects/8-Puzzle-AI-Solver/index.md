+++
title = '8 Puzzle AI Solver'
date = 2024-02-01T20:18:34+02:00
+++

## Introduction
* This project discusses the main differences between the different types of AI Search Algorithms like the completeness and the optimality of the algorithm 
* We used the 8-puzzle problem as an application that demonstrates these differences

## Depth First Search

* **Data Structure:** Stack
* **Completeness:** Not complete
* **Optimality:** Not optimal
* **Time:** O(b<sup>m</sup>)
* **Space:** O(b * m)
* **b** is the branching factor
* **m** is the maximum possible search tree depth
* DFS is good only when it comes to space complexity

## Breadth First Search

* **Data Structure:** Queue 
* **Completeness:** Complete
* **Optimality:** Optimal
* **Time:** O(b<sup>s</sup>)
* **Space:** O(b<sup>s</sup>)
* **b** is the branching factor
* **s** is the level where the goal state is present
* BFS is not very well dealing with space
* BFS can find the optimal solution only if all costs are equal to one

## A* Search with Manhattan heuristic

* **Data Structure:** Priority Queue
* **Completeness:** Complete
* **Optimality:** Optimal
* A* Search Always finds the optimal solution
* A* Search considers the past cost and the upcoming heuristic cost
* Manhattan distance = |x<sub>1</sub> - x<sub>2</sub>| + |y<sub>1</sub> - y<sub>2</sub>|

## A* Search with Euclidean heuristic

* **Data Structure:** Priority Queue
* **Completeness:** Complete
* **Optimality:** Optimal
* A* Search Always finds the optimal solution
* A* Search considers the past cost and the upcoming heuristic cost
* Euclidean distance = &radic;((x<sub>1</sub> - x<sub>2</sub>)<sup>2</sup> + (y<sub>1</sub> - y<sub>2</sub>)<sup>2</sup>)

## Test Cases & Comparisons
### Test #1 
**Input:** 1 2 3 4 5 6 8 7 0 _- Unsolvable test case -_

|          | DFS     | BFS     | A* M    | A* E    |
|----------|---------|---------|---------|---------|
| Cost     |         |         |         |         |
| Depth    | 66906   | 31      | 34      | 32      |
| Ex nodes | 181440  | 181440  | 181440  | 181440  |
| Time     | 0.38774 | 0.67555 | 1.06707 | 1.32079 |

### Test #2 
**Input:** 1 2 5 3 4 0 6 7 8

|          | DFS | BFS | A* M | A* E |
|----------|-----|-----|------|------|
| Cost     | 157 | 3   | 3    | 3    |
| Depth    | 157 | 3   | 3    | 3    |
| Ex nodes | 158 | 10  | 4    | 4    |
| Time     | 0.0 | 0.0 | 0.0  | 0.0  |

### Test #3 
**Input:** 1 0 2 7 5 4 8 6 3

|          | DFS     | BFS     | A* M    | A* E    |
|----------|---------|---------|---------|---------|
| Cost     | 17881   | 23      | 23      | 23      |
| Depth    | 17881   | 24      | 23      | 23      |
| Ex nodes | 18601   | 122117  | 2714    | 4306    |
| Time     | 0.03459 | 0.42513 | 0.01753 | 0.03551 |

### Test #4 
**Input:** 8 0 6 5 4 7 2 3 1

|          | DFS     | BFS     | A* M    | A* E    |
|----------|---------|---------|---------|---------|
| Cost     | 57329   | 31      | 31      | 31      |
| Depth    | 57329   | 31      | 31      | 31      |
| Ex nodes | 67802   | 181439  | 16582   | 35007   |
| Time     | 0.13396 | 0.77618 | 0.10551 | 0.28074 |

### Test #5
**Input:** 1 4 2 6 5 8 7 3 0

|          | DFS     | BFS   | A* M | A* E |
|----------|---------|-------|------|------|
| Cost     | 51618   | 8     | 8    | 8    |
| Depth    | 51618   | 9     | 8    | 8    |
| Ex nodes | 59137   | 203   | 13   | 13   |
| Time     | 0.11829 | 0.001 | 0.0  | 0.0  |

## Observation

* DFS has the longest paths and the least time in case of no solution

* A* Manhattan time is less than time of A* Euclidean
  * Reasoning: that Manhattan distance and Euclidean are both admissible but Manhattan distance is larger than Euclidean distance

* A* Manhattan/Euclidean heuristics decreases the number of expanded nodes so much than in BFS
