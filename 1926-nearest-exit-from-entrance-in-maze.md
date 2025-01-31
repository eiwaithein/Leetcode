# 1926 - Nearest Exit from Entrance in Maze

You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [entrancerow, entrancecol]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return _the **number of steps** in the shortest path from the_ `entrance` _to the nearest exit, or_ `-1` _if no such path exists_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

<pre><code><strong>Input: maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
</strong><strong>Output: 1
</strong><strong>Explanation: There are 3 exits in this maze at [1,0], [0,2], and [2,3].
</strong>Initially, you are at the entrance cell [1,2].
- You can reach [1,0] by moving 2 steps left.
- You can reach [0,2] by moving 1 step up.
It is impossible to reach [2,3] from the entrance.
Thus, the nearest exit is [0,2], which is 1 step away.
</code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

<pre><code><strong>Input: maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
</strong><strong>Output: 2
</strong><strong>Explanation: There is 1 exit in this maze at [1,2].
</strong>[1,0] does not count as an exit since it is the entrance cell.
Initially, you are at the entrance cell [1,0].
- You can reach [1,2] by moving 2 steps right.
Thus, the nearest exit is [1,2], which is 2 steps away.
</code></pre>

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

<pre><code><strong>Input: maze = [[".","+"]], entrance = [0,0]
</strong><strong>Output: -1
</strong><strong>Explanation: There are no exits in this maze.
</strong></code></pre>

&#x20;

**Constraints:**

* `maze.length == m`
* `maze[i].length == n`
* `1 <= m, n <= 100`
* `maze[i][j]` is either `'.'` or `'+'`.
* `entrance.length == 2`
* `0 <= entrancerow < m`
* `0 <= entrancecol < n`
* `entrance` will always be an empty cell.

#### Problem Explanation

You are given an `m x n` maze represented by a 2D array. Each cell is either a wall ('+') or a path ('.'). You are also given the entrance position in the maze. Your task is to find the nearest exit from the entrance. An exit is any empty cell that is not the entrance and is located on the border of the maze. You can move up, down, left, or right.

#### Solution Approach

We'll use Breadth-First Search (BFS) to traverse the maze starting from the entrance, exploring all possible paths to find the nearest exit. BFS is suitable here as it finds the shortest path in an unweighted grid.

#### Swift Code Implementation

swift

```swift
func nearestExit(_ maze: [[Character]], _ entrance: [Int]) -> Int {
    let rows = maze.count
    let cols = maze[0].count
    var maze = maze
    var queue: [(Int, Int, Int)] = [(entrance[0], entrance[1], 0)]
    let directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    while !queue.isEmpty {
        let (x, y, distance) = queue.removeFirst()
        
        for (dx, dy) in directions {
            let nx = x + dx
            let ny = y + dy
            
            if nx >= 0 && nx < rows && ny >= 0 && ny < cols && maze[nx][ny] == "." {
                if (nx == 0 || nx == rows - 1 || ny == 0 || ny == cols - 1) && (nx != entrance[0] || ny != entrance[1]) {
                    return distance + 1
                }
                
                maze[nx][ny] = "+"
                queue.append((nx, ny, distance + 1))
            }
        }
    }
    
    return -1
}
```

#### Time Complexity

* **Time Complexity**: O(m×n), where m is the number of rows and n is the number of columns. This complexity arises as we might visit each cell in the maze once.

#### Space Complexity

* **Space Complexity**: O(m×n) due to the additional space used by the queue to store the positions and distances during the BFS traversal.

#### Summary Bullet Points

* **Initialization**: Initialize a queue with the entrance position and set up directions for movement (up, down, left, right).
* **BFS Traversal**: Use BFS to explore all possible paths from the entrance.
* **Exit Check**: If an exit is found (a path on the border that's not the entrance), return the distance.
* **Mark Visited**: Mark cells as visited to avoid revisiting.
* **Return Result**: If no exit is reachable, return `-1`.
