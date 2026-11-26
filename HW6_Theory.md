# Part A: DFS Timestamps & Ordering

## 1. Why DFS, Not BFS, For This Job:

BFS orders vertices by distance from the start and moves on as soon as a vertex is dequeued, so there is no point where a vertex is "finished" after everything downstream of it is done, and a course can even land on the same level as its prerequisite. DFS can't finish a vertex until every neighboring course from it has finished, so a prerequisite always finishes after the courses it unlocks.

## 2. Visual Tracing - DFS Discover/Finish Times:

### Graph

```
         +------> CS215 ------+
         |                    |
         |                    v
CS115 ---+                  CS311 ------> CS411
         |                    ^
         |                    |
         +------> CS258 ------+
```

### Trace starting at CS115

| Time | Event          |
| ---- | -------------- |
| 1    | Discover CS115 |
| 2    | Discover CS215 |
| 3    | Discover CS311 |
| 4    | Discover CS411 |
| 5    | Finish CS411   |
| 6    | Finish CS311   |
| 7    | Finish CS215   |
| 8    | Discover CS258 |
| 9    | Finish CS258   |
| 10   | Finish CS115   |

### Discovery / finish summary

| Course | Discovery (d) | Finish (f) |
| ------ | ------------- | ---------- |
| CS115  | 1             | 10         |
| CS215  | 2             | 7          |
| CS258  | 8             | 9          |
| CS311  | 3             | 6          |
| CS411  | 4             | 5          |

## 3. From Finish Times to Topological Order:

### Rule

The rule for DFS is to order them by the highest finish count -> lowest finish count. This gurantees that the prerequisite courses always finish last.

### Applied to Quesiton 2 Table:

CS115 -> CS258 -> CS215 -> CS311 -> CS411

## 4. Cycle via Edge Classification:

A plain "already visited" check isn't enough because a visited vertex can be either still on the current path or already finished. If we look at the Question 2 trace, CS258 → CS311 points to an already-visited vertex, but CS311 finished at time 6 and isn't on the current path, so it's a normal DAG edge (311 just has two prerequisites), not a cycle. If we just checked if CS311 has been visited this would create a back edge where we don't want one. So, an already visited check isn't enough to determine whether or not an edge is a back edge or normal DAG edge.
