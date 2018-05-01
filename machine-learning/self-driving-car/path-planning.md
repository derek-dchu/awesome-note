## Planning Problem
Given: map, starting loc, goal loc, cost
Goal: find the minimum cost path

### Algorithms
#### A*
BFS with priority on cost + heuristic function

The heuristic function usually set to be distance to goal loc or `h = dist(g)`

#### Dynamic Programming
DP actually finds the best path and multiple plans at different positions.

DP generates a **policy map**, any given position of policy map can follow policies (best action to take) to reach goal loc.


