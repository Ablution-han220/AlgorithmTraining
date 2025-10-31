# Graph
## Theory
### 图的种类
有向图 和 无向图
### 度
无向图中有几条边连接该节点，该节点就有几度   

在有向图中，每个节点有出度和入度。

出度：从该节点出发的边的个数。

入度：指向该节点边的个数。
### 连通性
在图中表示节点的连通情况，我们称之为连通性。
### 连通图
在无向图中，任何两个节点都是可以到达的，我们称之为连通图
### 强连通图
在有向图中，任何两个节点是可以相互到达的，我们称之为 强连通图。
### 图的构造
#### 邻接矩阵：
使用 二维数组来表示图结构。 邻接矩阵是从节点的角度来表示图，有多少节点就申请多大的二维数组。

例如： grid[2][5] = 6，表示 节点 2 连接 节点5 为有向图，节点2 指向 节点5，边的权值为6。

如果想表示无向图，即：grid[2][5] = 6，grid[5][2] = 6，表示节点2 与 节点5 相互连通，权值为6。

邻接矩阵的优点：

表达方式简单，易于理解
检查任意两个顶点间是否存在边的操作非常快
适合稠密图，在边数接近顶点数平方的图中，邻接矩阵是一种空间效率较高的表示方法。
缺点：

遇到稀疏图，会导致申请过大的二维数组造成空间浪费 且遍历 边 的时候需要遍历整个n * n矩阵，造成时间浪费

#### 邻接表： 
使用 数组 + 链表的方式来表示。 邻接表是从边的数量来表示图，有多少边 才会申请对应大小的链表。

邻接表的优点：

对于稀疏图的存储，只需要存储边，空间利用率高
遍历节点连接情况相对容易
缺点：

检查任意两个节点间是否存在边，效率相对低，需要 O(V)时间，V表示某节点连接其他节点的数量。
实现相对复杂，不易理解

### 图的遍历方式
图的遍历方式基本是两大类：

深度优先搜索（dfs）
```c++
void dfs(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本节点所连接的其他节点) {
        处理节点;
        dfs(图，选择的节点); // 递归
        回溯，撤销处理结果
    }
}
```
广度优先搜索（bfs）
```c++
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 表示四个方向
// grid 是地图，也就是一个二维数组
// visited标记访问过的节点，不要重复访问
// x,y 表示开始搜索节点的下标
void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> que; // 定义队列
    que.push({x, y}); // 起始节点加入队列
    visited[x][y] = true; // 只要加入队列，立刻标记为访问过的节点
    while(!que.empty()) { // 开始遍历队列里的元素
        pair<int ,int> cur = que.front(); que.pop(); // 从队列取元素
        int curx = cur.first;
        int cury = cur.second; // 当前节点坐标
        for (int i = 0; i < 4; i++) { // 开始想当前节点的四个方向左右上下去遍历
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1]; // 获取周边四个方向的坐标
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 坐标越界了，直接跳过
            if (!visited[nextx][nexty]) { // 如果节点没被访问过
                que.push({nextx, nexty});  // 队列添加该节点为下一轮要遍历的节点
                visited[nextx][nexty] = true; // 只要加入队列立刻标记，避免重复访问
            }
        }
    }

}
```

## Part.3
1254. Number of Closed Islands
```c++
class Solution {
public:
    //BFS
    void bfs(vector<vector<int>>& grid, int x, int y) {
        queue<pair<int,int>> q;
        q.push({x, y});
        grid[x][y] = 1; //mark visited
        while(!q.empty()) {
            pair<int, int> cur = q.front();
            q.pop();
            for(int i = 0; i < 4; ++i) {
                int nextx = cur.first + dir[i][0];
                int nexty = cur.second + dir[i][1];
                if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
                if(grid[nextx][nexty] == 0) {
                    q.push({nextx,nexty});
                    grid[nextx][nexty] = 1;
                }
            }            
        }
    }
    int closedIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        int count = 0;
        // Flood-fill all lands touching boundary (not closed)
        for(int i = 0; i < n; ++i) {
            if(grid[i][0] == 0) bfs(grid, i, 0);
            if(grid[i][m - 1] == 0) bfs(grid, i, m - 1);
        }
        for(int j = 0; j < m; ++j) {
            if(grid[0][j] == 0) bfs(grid, 0, j);
            if(grid[n - 1][j] == 0) bfs(grid, n - 1, j);
        }
        // Count remaining closed islands
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(grid[i][j] == 0) {
                    bfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    //DFS
    // void dfs(vector<vector<int>>& grid, int x, int y) {
    //     if(grid[x][y] == 1) return;
    //     grid[x][y] = 1; //mark visited
    //     for(int i = 0; i < 4; ++i) {
    //         int nextx = x + dir[i][0];
    //         int nexty = y + dir[i][1];
    //         if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
    //         dfs(grid, nextx, nexty);
    //     }
    // }
    // int closedIsland(vector<vector<int>>& grid) {
    //     int n = grid.size();
    //     int m = grid[0].size();
    //     int count = 0;
    //     // Flood-fill all lands touching boundary (not closed)
    //     for(int i = 0; i < n; ++i) {
    //         if(grid[i][0] == 0) dfs(grid, i, 0);
    //         if(grid[i][m - 1] == 0) dfs(grid, i, m - 1);
    //     }
    //     for(int j = 0; j < m; ++j) {
    //         if(grid[0][j] == 0) dfs(grid, 0, j);
    //         if(grid[n - 1][j] == 0) dfs(grid, n - 1, j);
    //     }
    //     // Count remaining closed islands
    //     for(int i = 0; i < n; ++i) {
    //         for(int j = 0; j < m; ++j) {
    //             if(grid[i][j] == 0) {
    //                 dfs(grid, i, j);
    //                 count++;
    //             }
    //         }
    //     }
    //     return count;
    // }
private:
    int dir[4][2] = {{0,1}, {1,0}, {0, -1}, {-1,0}};
};
Time Complexity: O(m * n) 
Space Complexity: O(m *n )
```
 
卡码网KamaCoder
101. 孤岛的总面积
```c++
#include<vector>
#include<iostream>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
void dfs(vector<vector<int>>& grid, int x, int y) {
        if(grid[x][y] == 0) return;
        grid[x][y] = 0; //mark visited
        for(int i = 0; i < 4; ++i) {
            int nextx = x + dir[i][0];
            int nexty = y + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            dfs(grid, nextx, nexty);
        }
    }

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    int count = 0;
    for(int i = 0; i < n; ++i) {
        if(grid[i][0] == 1) dfs(grid, i, 0);
        if(grid[i][m - 1] == 1) dfs(grid, i, m - 1);
    }
    for(int j = 0; j < m; ++j) {
        if(grid[0][j] == 1) dfs(grid, 0, j);
        if(grid[n - 1][j] == 1) dfs(grid, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 1) {
                count++;
            }
        }
    }
    cout << count << endl;
    return 0;
}

BFS
#include<vector>
#include<iostream>
#include<queue>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
void bfs(vector<vector<int>>& grid, int x, int y) {
    queue<pair<int,int>> q;
    q.push({x, y});
    grid[x][y] = 0; //mark visited
    while(!q.empty()) {
        pair<int, int> cur = q.front();
        q.pop();
        for(int i = 0; i < 4; ++i) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if(grid[nextx][nexty] == 1) {
                q.push({nextx,nexty});
                grid[nextx][nexty] = 0;
            }
        }            
    }
}
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    int count = 0;
    for(int i = 0; i < n; ++i) {
        if(grid[i][0] == 1) bfs(grid, i, 0);
        if(grid[i][m - 1] == 1) bfs(grid, i, m - 1);
    }
    for(int j = 0; j < m; ++j) {
        if(grid[0][j] == 1) bfs(grid, 0, j);
        if(grid[n - 1][j] == 1) bfs(grid, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 1) {
                count++;
            }
        }
    }
    cout << count << endl;
    return 0;
}
Time Complexity: O(m*n)
Space Complexity: O(m*n)
```

130. Surrounded Regions
```c++
class Solution {
public:
    //BFS
    void bfs(vector<vector<char>>& board, int x, int y) {
        queue<pair<int,int>> q;
        q.push({x,y});
        board[x][y] = 'P';
        while(!q.empty()) {
            pair<int,int> cur = q.front();
            q.pop();
            for(int i = 0; i < 4; ++i) {
                int nextx = cur.first + dir[i][0];
                int nexty = cur.second + dir[i][1];
                if(nextx < 0 || nextx >= board.size() || nexty < 0 || nexty >= board[0].size()) continue;
                if(board[nextx][nexty] == 'O') {
                    q.push({nextx, nexty});
                    board[nextx][nexty] = 'P';
                }
            }
        }
    }
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();
        for(int i = 0; i < n; ++i) {
            if(board[i][0] == 'O') bfs(board, i, 0);
            if(board[i][m - 1] == 'O') bfs(board, i, m - 1);
        }
        for(int j = 0; j < m; ++j) {
            if(board[0][j] == 'O') bfs(board, 0, j);
            if(board[n - 1][j] == 'O') bfs(board, n - 1, j);
        }
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(board[i][j] == 'O') board[i][j] = 'X';
                if(board[i][j] == 'P') board[i][j] = 'O';
            }
        }
        return;
    }
    // DFS
    // void dfs(vector<vector<char>>& board, int x, int y) {
    //     if(board[x][y] == 'P' || board[x][y] == 'X') return;
    //     board[x][y] = 'P';
    //     for(int i = 0; i < 4; ++i) {
    //         int nextx = x + dir[i][0];
    //         int nexty = y + dir[i][1];
    //         if(nextx < 0 || nextx >= board.size() || nexty < 0 || nexty >= board[0].size()) continue;
    //         dfs(board, nextx, nexty);
    //     }
    // }
    // void solve(vector<vector<char>>& board) {
    //     int n = board.size();
    //     int m = board[0].size();
    //     for(int i = 0; i < n; ++i) {
    //         if(board[i][0] == 'O') dfs(board, i, 0);
    //         if(board[i][m - 1] == 'O') dfs(board, i, m - 1);
    //     }
    //     for(int j = 0; j < m; ++j) {
    //         if(board[0][j] == 'O') dfs(board, 0, j);
    //         if(board[n - 1][j] == 'O') dfs(board, n - 1, j);
    //     }
    //     for(int i = 0; i < n; ++i) {
    //         for(int j = 0; j < m; ++j) {
    //             if(board[i][j] == 'O') board[i][j] = 'X';
    //             if(board[i][j] == 'P') board[i][j] = 'O';
    //         }
    //     }
    //     return;
    // }
private:
    int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
};
Time Complexity: O(m * n) 
Space Complexity: O(m *n )
```

卡码网KamaCoder
102. 沉没孤岛
```c++
DFS
#include<vector>
#include<iostream>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
void dfs(vector<vector<int>>& grid, int x, int y) {
        if(grid[x][y] == 0 || grid[x][y] == 2) return;
        grid[x][y] = 2; //mark visited
        for(int i = 0; i < 4; ++i) {
            int nextx = x + dir[i][0];
            int nexty = y + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            dfs(grid, nextx, nexty);
        }
    }

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    int count = 0;
    for(int i = 0; i < n; ++i) {
        if(grid[i][0] == 1) dfs(grid, i, 0);
        if(grid[i][m - 1] == 1) dfs(grid, i, m - 1);
    }
    for(int j = 0; j < m; ++j) {
        if(grid[0][j] == 1) dfs(grid, 0, j);
        if(grid[n - 1][j] == 1) dfs(grid, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 1) grid[i][j] = 0;
            if(grid[i][j] == 2) grid[i][j] = 1;
        }
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cout<< grid[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}

BFS
#include<vector>
#include<queue>
#include<iostream>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
void bfs(vector<vector<int>>& grid, int x, int y) {
    queue<pair<int,int>> q;
    q.push({x, y});
    grid[x][y] = 2; //mark visited
    while(!q.empty()) {
        pair<int, int> cur = q.front();
        q.pop();
        for(int i = 0; i < 4; ++i) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if(grid[nextx][nexty] == 1) {
                q.push({nextx,nexty});
                grid[nextx][nexty] = 2;
            }
        }            
    }
}
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    int count = 0;
    for(int i = 0; i < n; ++i) {
        if(grid[i][0] == 1) bfs(grid, i, 0);
        if(grid[i][m - 1] == 1) bfs(grid, i, m - 1);
    }
    for(int j = 0; j < m; ++j) {
        if(grid[0][j] == 1) bfs(grid, 0, j);
        if(grid[n - 1][j] == 1) bfs(grid, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 1) grid[i][j] = 0;
            if(grid[i][j] == 2) grid[i][j] = 1;
        }
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cout<< grid[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
Time Complexity: O(m*n)
Space Complexity: O(m*n)
```

417. Pacific Atlantic Water Flow
```c++
class Solution {
public:
   void bfs(const vector<vector<int>>& heights, vector<vector<bool>>& visited, int x, int y) {
        queue<pair<int,int>> q;
        q.push({x, y});
        visited[x][y] = true;
        while(!q.empty()) {
            pair<int,int> cur = q.front();
            q.pop();
            for(int i = 0; i < 4; ++i) {
                int nextx = cur.first + dir[i][0];
                int nexty = cur.second + dir[i][1];
                if(nextx < 0 || nextx >= heights.size() || nexty < 0 || nexty >= heights[0].size()) continue;
                // if(heights[cur.first][cur.second] > heights[nextx][nexty]) continue;
                if(heights[nextx][nexty] < heights[cur.first][cur.second] || visited[nextx][nexty]) continue;
                q.push({nextx, nexty});
                visited[nextx][nexty] = true;
            }
        }
    }
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int n = heights.size();
        int m = heights[0].size();
        vector<vector<int>> res;
        vector<vector<bool>> pacific(n, vector<bool>(m, false));
        vector<vector<bool>> atlantic(n, vector<bool>(m, false));
        for(int i = 0; i < n; ++i) {
            bfs(heights, pacific, i, 0);
            bfs(heights, atlantic, i, m - 1);
        }
        for(int j = 0; j < m; ++j) {
            bfs(heights, pacific, 0, j);
            bfs(heights, atlantic, n - 1, j);
        }
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(pacific[i][j] && atlantic[i][j]) {
                    res.emplace_back(vector<int>{i, j});
                }
            }
        }
        return res;
    }   
    // void dfs(const vector<vector<int>>& heights, vector<vector<bool>>& visited, int x, int y) {
    //     if(visited[x][y]) return;
    //     visited[x][y] = true;
    //     for(int i = 0; i < 4; ++i) {
    //         int nextx = x + dir[i][0];
    //         int nexty = y + dir[i][1];
    //         if(nextx < 0 || nextx >= heights.size() || nexty < 0 || nexty >= heights[0].size()) continue;
    //         if(heights[x][y] > heights[nextx][nexty]) continue;
    //         dfs(heights, visited, nextx, nexty);
    //     }
    // }
    // vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
    //     int n = heights.size();
    //     int m = heights[0].size();
    //     vector<vector<int>> res;
    //     vector<vector<bool>> pacific(n, vector<bool>(m, false));
    //     vector<vector<bool>> atlantic(n, vector<bool>(m, false));
    //     for(int i = 0; i < n; ++i) {
    //         dfs(heights, pacific, i, 0);
    //         dfs(heights, atlantic, i, m - 1);
    //     }
    //     for(int j = 0; j < m; ++j) {
    //         dfs(heights, pacific, 0, j);
    //         dfs(heights, atlantic, n - 1, j);
    //     }
    //     for(int i = 0; i < n; ++i) {
    //         for(int j = 0; j < m; ++j) {
    //             if(pacific[i][j] && atlantic[i][j]) {
    //                 res.emplace_back(vector<int>{i, j});
    //             }
    //         }
    //     }
    //     return res;
    // }
private:
    int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
};
Time Complexity: O(m*n)
Space Complexity: O(m*n)
```

卡码网KamaCoder
103. 高山流水
```c++
DFS
#include<iostream>
#include<vector>
using namespace std;
 
int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
void dfs(const vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    if(visited[x][y]) return;
    visited[x][y] = true;
    for(int i = 0; i < 4; ++i) {
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
        if(grid[x][y] > grid[nextx][nexty]) continue;
        dfs(grid, visited, nextx, nexty);
    }
}
 
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cin >> grid[i][j];
        }
    }
    vector<vector<bool>> firstborder(n, vector<bool>(m, false));
    vector<vector<bool>> secondborder(n, vector<bool>(m, false));
    // left border and right border
    for(int i = 0; i < n; ++i) {
        dfs(grid, firstborder, i, 0);
        dfs(grid, secondborder, i, m - 1);
    }
    // top border and bottom border
    for(int j = 0; j < m; ++j) {
        dfs(grid, firstborder, 0, j);
        dfs(grid, secondborder, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(firstborder[i][j] && secondborder[i][j]) {
                cout<< i << " " << j << endl;
            }
        }
    }
    return 0;
}

BFS
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
 
int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
void bfs(const vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> q;
    q.push({x, y});
    visited[x][y] = true;
    while(!q.empty()) {
        pair<int, int> cur = q.front();
        q.pop();
        for(int i = 0; i < 4; ++i) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if(grid[cur.first][cur.second] <= grid[nextx][nexty] && !visited[nextx][nexty]) {
                q.push({nextx, nexty});
                visited[nextx][nexty] = true;
            }
             
        }
    }
}
 
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cin >> grid[i][j];
        }
    }
    vector<vector<bool>> firstborder(n, vector<bool>(m, false));
    vector<vector<bool>> secondborder(n, vector<bool>(m, false));
    // left border and right border
    for(int i = 0; i < n; ++i) {
        bfs(grid, firstborder, i, 0);
        bfs(grid, secondborder, i, m - 1);
    }
    // top border and bottom border
    for(int j = 0; j < m; ++j) {
        bfs(grid, firstborder, 0, j);
        bfs(grid, secondborder, n - 1, j);
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(firstborder[i][j] && secondborder[i][j]) {
                cout<< i << " " << j << endl;
            }
        }
    }
    return 0;
}
Time Complexity: O(m*n)
Space Complexity: O(m*n)
```

827. Making A Large Island
```c++
class Solution {
public:
    void bfs(vector<vector<int>>& grid, int x, int y, int& size, int marker) {
        queue<pair<int,int>> q;
        q.push({x, y});
        grid[x][y] = marker;
        size++;
        while(!q.empty()) {
            pair<int, int> cur = q.front();
            q.pop();
            for(int i = 0; i < 4; ++i) {
                int nextx = cur.first + dir[i][0];
                int nexty = cur.second + dir[i][1];
                if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
                if(grid[nextx][nexty] != 1) continue;
                q.push({nextx, nexty});
                size++;
                grid[nextx][nexty] = marker;
            } 
        }
    }
    // void dfs(vector<vector<int>>& grid, int x, int y, int& size, int marker) {
    //     if(grid[x][y] != 1) return;
    //     grid[x][y] = marker;
    //     size++;
    //     for(int i = 0; i < 4; ++i) {
    //         int nextx = x + dir[i][0];
    //         int nexty = y + dir[i][1];
    //         if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
    //         dfs(grid, nextx, nexty, size, marker);
    //     }    
    // }
    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        unordered_map<int,int> islandArea;
        int islandId = 2;
        bool isAllLand = true;
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(grid[i][j] == 0) isAllLand = false;
                if(grid[i][j] == 1) {
                    int size = 0;
                    bfs(grid, i, j, size, islandId);
                    islandArea[islandId] = size;
                    islandId++; 
                }
            }
        }
        int res = 0;
        if(isAllLand) return n*m;
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                unordered_set<int> visited;
                int count = 1;
                if(grid[i][j] == 0) {
                    for(int k = 0; k < 4; ++k) {
                        int x = i + dir[k][0];
                        int y = j + dir[k][1];
                        if(x < 0 || x >= n || y < 0 || y >= m) continue;
                        if(visited.count(grid[x][y])) continue;
                        count += islandArea[grid[x][y]];
                        visited.insert(grid[x][y]);
                    }
                }
                res = max(res, count);
            }
        }
        return res;
    }

private:
    int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
};
Time Complexity: O(m*n)
Space Complexity: O(m*n)
```

卡码网KamaCoder
104. 建造最大岛屿
```c++
DFS
#include<iostream>
#include<vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;
 
int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
void dfs(vector<vector<int>>& grid, int x, int y, int& size, int marker) {
    if(grid[x][y] != 1) return;
    grid[x][y] = marker;
    size++;
    for(int i = 0; i < 4; ++i) {
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
        dfs(grid, nextx, nexty, size, marker);
    }    
}
 
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cin >> grid[i][j];
        }
    }
    unordered_map<int,int> islandArea;
    int islandId = 2;
    bool isAllLand = true;
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 0) isAllLand = false;
            if(grid[i][j] == 1) {
                int size = 0;
                dfs(grid, i, j, size, islandId);
                islandArea[islandId] = size;
                islandId++; 
            }
        }
    }
    int res = 0;
    if(isAllLand) {
        cout << n*m << endl;
        return 0;
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            unordered_set<int> visited;
            int count = 1;
            if(grid[i][j] == 0) {
                for(int k = 0; k < 4; ++k) {
                    int x = i + dir[k][0];
                    int y = j + dir[k][1];
                    if(x < 0 || x >= n || y < 0 || y >= m) continue;
                    if(visited.count(grid[x][y])) continue;
                    count += islandArea[grid[x][y]];
                    visited.insert(grid[x][y]);
                }
            }
            res = max(res, count);
        }
    }
    cout<< res << endl;
    return 0;
}

BFS
#include<iostream>
#include<vector>
#include<queue>
#include <unordered_map>
#include <unordered_set>
using namespace std;
 
int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
void bfs(vector<vector<int>>& grid, int x, int y, int& size, int marker) {
    queue<pair<int,int>> q;
    q.push({x, y});
    grid[x][y] = marker;
    size++;
    while(!q.empty()) {
        pair<int, int> cur = q.front();
        q.pop();
        for(int i = 0; i < 4; ++i) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if(nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if(grid[nextx][nexty] != 1) continue;
            q.push({nextx, nexty});
            size++;
            grid[nextx][nexty] = marker;
        } 
    }
}
 
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cin >> grid[i][j];
        }
    }
    unordered_map<int,int> islandArea;
    int islandId = 2;
    bool isAllLand = true;
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 0) isAllLand = false;
            if(grid[i][j] == 1) {
                int size = 0;
                bfs(grid, i, j, size, islandId);
                islandArea[islandId] = size;
                islandId++; 
            }
        }
    }
    int res = 0;
    if(isAllLand) {
        cout << n*m << endl;
        return 0;
    }
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            unordered_set<int> visited;
            int count = 1;
            if(grid[i][j] == 0) {
                for(int k = 0; k < 4; ++k) {
                    int x = i + dir[k][0];
                    int y = j + dir[k][1];
                    if(x < 0 || x >= n || y < 0 || y >= m) continue;
                    if(visited.count(grid[x][y])) continue;
                    count += islandArea[grid[x][y]];
                    visited.insert(grid[x][y]);
                }
            }
            res = max(res, count);
        }
    }
    cout<< res << endl;
    return 0;
}
```