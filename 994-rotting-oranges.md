# 994 - Rotting Oranges

## Question

You are given an `m x n` `grid` where each cell can have one of three values:

* `0` representing an empty cell,
* `1` representing a fresh orange, or
* `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

<pre><code><strong>Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
</strong><strong>Output: 4
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
</strong><strong>Output: -1
</strong><strong>Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: grid = [[0,2]]
</strong><strong>Output: 0
</strong><strong>Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
</strong></code></pre>

**Constraints:**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 10`
* `grid[i][j]` is `0`, `1`, or `2`.

## Solution

#### Problem Explanation

The problem involves a 2D grid where:

* `0` represents an empty cell.
* `1` represents a fresh orange.
* `2` represents a rotten orange.

Every minute, any fresh orange that is adjacent (4 directions: up, down, left, right) to a rotten orange becomes rotten. The goal is to determine the minimum number of minutes that must pass until no cell has a fresh orange. If it's impossible to rot all the fresh oranges, return `-1`.

#### Solution Approach

We'll use Breadth-First Search (BFS) to simulate the rotting process, starting from all initially rotten oranges and rotting their neighbors in subsequent rounds.

#### Swift Code Implementation

```swift
func orangesRotting(_ grid: [[Int]]) -> Int {
    var grid = grid
    let rows = grid.count
    let cols = grid[0].count
    var queue: [(Int, Int)] = []
    var freshOranges = 0
    
    // Initialize the queue with all initially rotten oranges and count fresh oranges
    for r in 0..<rows {
        for c in 0..<cols {
            if grid[r][c] == 2 {
                queue.append((r, c))
            } else if grid[r][c] == 1 {
                freshOranges += 1
            }
        }
    }
    
    guard freshOranges > 0 else { return 0 }
    
    let directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    var minutes = 0
    
    while !queue.isEmpty {
        minutes += 1
        var nextQueue: [(Int, Int)] = []
        
        for (r, c) in queue {
            for (dr, dc) in directions {
                let nr = r + dr
                let nc = c + dc
                
                if nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == 1 {
                    grid[nr][nc] = 2
                    freshOranges -= 1
                    nextQueue.append((nr, nc))
                }
            }
        }
        queue = nextQueue
    }
    
    return freshOranges == 0 ? minutes - 1 : -1
}
```

#### Time Complexity

* **Time Complexity**: O(N×M) where N is the number of rows and MM is the number of columns. We visit each cell at most once during the BFS traversal.

#### Space Complexity

* **Space Complexity**: O(N×M) due to the additional space used by the queue to store the positions of the rotten oranges.

#### Summary Bullet Points

* **Initialization**: Initialize a queue with all initially rotten oranges and count the number of fresh oranges.
* **BFS Traversal**: Use BFS to rot adjacent fresh oranges, decreasing their count.
* **Minute Tracking**: Keep track of the time (minutes) to rot all reachable fresh oranges.
* **Edge Case Handling**: If no fresh oranges are left at the end, return the time; otherwise, return `-1`.

#### Debugging Table

This table captures the state of the grid, the queue, the fresh orange count, and the current minute during each iteration of the BFS traversal.

initial grid value =

```
[[2, 1, 2],
 [1, 1, 1],
 [0, 1, 1]]
```

| Minute | Queue                     | Fresh Oranges | Grid State                                           |
| ------ | ------------------------- | ------------- | ---------------------------------------------------- |
| 0      | \[(0, 2), (1, 0)]         | 5             | <p>[[2, 1, 2],</p><p>[2, 1, 1],</p><p>[0, 1, 1]]</p> |
| 1      | \[(0, 1), (1, 1)]         | 3             | <p>[[2, 2, 2],</p><p>[2, 2, 1],</p><p>[0, 1, 1]]</p> |
| 2      | \[(0, 1), (1, 2), (2, 1)] | 1             | <p>[[2, 2, 2],</p><p>[2, 2, 2],</p><p>[0, 2, 1]]</p> |
| 3      | \[(2, 2)]                 | 0             | <p>[[2, 2, 2],</p><p>[2, 2, 2],</p><p>[0, 2, 2]]</p> |

#### Explanation

* **Minute 0**: Initially, the queue contains the positions of all rotten oranges: (0, 2) and (1, 0). There are 5 fresh oranges.
* **Minute 1**: After one minute, the fresh oranges adjacent to rotten oranges become rotten. The queue now contains (0, 1) and (1, 1). Fresh oranges count decreases to 3.
* **Minute 2**: After the second minute, the fresh oranges at positions (0, 1), (1, 2), and (2, 1) become rotten. The queue now contains (2, 1). Fresh oranges count decreases to 1.
* **Minute 3**: After the third minute, the last fresh orange at position (2, 2) becomes rotten. The queue contains (2, 2) and fresh oranges count is 0.

This table helps visualize how the grid changes over time and how the rotting process spreads through the grid. If at any point fresh oranges remain and cannot be reached by rotten ones, it indicates that not all fresh oranges can be rotted, and the function would return `-1`.
