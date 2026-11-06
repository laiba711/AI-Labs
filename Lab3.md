# Cell 6 (Misplaced Tiles Heuristic)
```python
def misplaced_tiles(state):
    """
    Count tiles that are not in their goal position.
    The blank (0) does NOT count as a misplaced tile.
    """
    count = 0
    for i in range(len(state)):
        if state[i] != 0 and state[i] != GOAL_STATE[i]:
            count += 1
    return count
```
# Cell 7 (Manhattan Distance Heuristic)
``` python
def manhattan_distance(state):
    """
    Sum of the horizontal + vertical distance of every tile
    (excluding the blank) from its goal position.
    """
    distance = 0
    for i, tile in enumerate(state):
        if tile != 0:
            current_row, current_col = divmod(i, 3)
            goal_index = GOAL_STATE.index(tile)
            goal_row, goal_col = divmod(goal_index, 3)
            distance += abs(current_row - goal_row) + abs(current_col - goal_col)
    return distance
```
# Cell 10 (A* Search Implementation)
```python
def a_star_search(start_state, heuristic_fn):
    """
    Returns a dict with:
        "path": list of actions from start to goal
        "cost": number of moves in the solution
        "nodes_expanded": total number of nodes popped from the frontier
    """
    counter = itertools.count()  # tie-breaker for equal f-values
    frontier = []
    heapq.heappush(frontier, (heuristic_fn(start_state), next(counter), 0, start_state, []))

    best_g = {start_state: 0}
    nodes_expanded = 0

    while frontier:
        f, _, g, state, path = heapq.heappop(frontier)
        nodes_expanded += 1

        if is_goal(state):
            return {"path": path, "cost": g, "nodes_expanded": nodes_expanded}

        if g > best_g.get(state, float('inf')):
            continue

        for neighbor_state, action in get_neighbors(state):
            new_g = g + 1  # each move costs 1

            if neighbor_state not in best_g or new_g < best_g[neighbor_state]:
                best_g[neighbor_state] = new_g
                new_f = new_g + heuristic_fn(neighbor_state)
                heapq.heappush(frontier, (new_f, next(counter), new_g, neighbor_state, path + [action]))

    return {"path": None, "cost": None, "nodes_expanded": nodes_expanded}
```
# Cell 17 (Section 5 Analysis)
1. **Table:**

| Scramble Depth | Misplaced Nodes | Manhattan Nodes |
| :--- | :--- | :--- |
| **5** | 7.2 | 5.8 |
| **10** | 45.6 | 14.4 |
| **15** | 382.0 | 38.6 |
| **20** | 3120.4 | 112.8 |

2. **Discussion (Why $h(n) = 0$ at Goal):**
Both heuristics must return 0 at the goal state because the goal state has 0 misplaced tiles and 0 Manhattan distance to itself. If $h(\text{GOAL}) > 0$, the heuristic would overestimate the remaining cost to reach the goal (which is 0), violating admissibility and potentially causing A* to return a non-optimal solution.

3. **Answers:**
   - **Gap Visibility & Widening:** The gap between Misplaced Tiles and Manhattan Distance becomes clearly visible around scramble depth 10. As depth increases, the state space grows exponentially. Because Manhattan Distance provides a tighter/higher lower bound closer to the actual cost, it prunes significantly more suboptimal branches, causing the node expansion gap to widen exponentially.
   - **Heuristic Dominance & Admissibility:** A higher admissible heuristic ($h_2(n) \ge h_1(n)$) does not violate admissibility as long as it never exceeds the true cost $h^*(n)$ (i.e., $h_1(n) \le h_2(n) \le h^*(n)$). We say $h_2$ dominates $h_1$. A higher heuristic value $f(n) = g(n) + h(n)$ allows A* to recognize non-promising paths much earlier, expanding fewer nodes to guarantee optimality.
   - **Impractical Depth Scaling:** When scramble depth is increased beyond 20–25 moves, Misplaced Tiles becomes impractically slow due to memory and time limits (expanding tens of thousands of nodes). Manhattan Distance does not hit the same wall at that depth and handles deeper scrambles much faster due to its superior search guidance.
