# 7.Compare BFS and DFS
| Algorithm | Path Length | Maximum Frontier |
| :--- | :--- | :--- |
| **BFS** | 8 | 3 |
| **DFS** | 12 | 4 |
1. **Which algorithm found the shorter path?**
   BFS found the shorter path because it explores the graph level-by-level.

2. **Which algorithm had the larger maximum frontier?**
   BFS had a larger maximum frontier because it expands nodes layer-by-layer, keeping more states in memory simultaneously.

3. **Why did the two algorithms produce different paths on the same maze?**
   BFS uses a Queue (FIFO) to explore nodes level-wise, while DFS uses a Stack (LIFO) to explore as deep as possible down a single path before backtracking.

4. **Does DFS always produce the shortest path? Why or why not?**
   No, DFS does not guarantee the shortest path because it commits to the first branch it explores deep down, regardless of path cost or length.

5. **Does BFS always produce the shortest path when every movement costs 1? Why?**
   Yes, BFS always guarantees the shortest path when edge costs are equal (1) because it reaches nodes with the minimum number of steps first.
# 8.Experiment 1-Change the neighbor order
**Question:**
Did the DFS path change? Did BFS's path (Section 4) depend on this ordering the same way? Why or why not?

**Answer:**
Yes, the DFS path changed because DFS uses a Stack (LIFO), so changing the order of neighbor directions directly alters which path it explores first. BFS's path length does not depend on the ordering in the same way because it explores breadth-first level-by-level, ensuring it still finds the shortest path regardless of direction order.
# 9.Experiment 2-Change start and goal
**Table Results (Start, Goal Pairs):**

| Start | Goal | BFS Length | BFS Max Frontier | DFS Length | DFS Max Frontier |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `(0, 0)` | `(2, 2)` | `4` | `3` | `6` | `3` |
| `(0, 0)` | `(0, 4)` | `4` | `2` | `4` | `2` |
| `(2, 0)` | `(4, 4)` | `6` | `3` | `8` | `3` |

**Questions & Answers:**

1. **Does BFS's path length ever change if you re-run it with the same start/goal? Does DFS's?**
   No, BFS's path length never changes for the same start/goal because it always finds the shortest path. DFS's path length also remains the same as long as the direction order and maze structure do not change.

2. **As the distance between start and goal grows, what happens to the maximum frontier size for each algorithm?**
   As the distance grows, the maximum frontier size for both algorithms generally increases because more nodes need to be stored in memory during the search process.

3. **Can you find a (start, goal) pair where BFS and DFS return the same path? What does that tell you about the maze structure between those two points?**
   Yes, if there is only a single clear corridor or straight line between the start and goal with no alternate branches, both BFS and DFS will return the exact same path. This indicates a linear or unbranched maze structure between those points.

4. **What happens if goal is unreachable from start (e.g. sealed off by walls)? Try it — what do bfs() and dfs() return?**
   If the goal is completely unreachable (sealed off by walls), both `bfs()` and `dfs()` will return `None` for the path.

