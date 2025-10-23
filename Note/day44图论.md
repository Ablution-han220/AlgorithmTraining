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

## Part.1
797. All Paths From Source to Target
```c++
class Solution {
public:
    // BFS
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<int> path;
        vector<vector<int>> res;
        queue<vector<int>> q;
        q.push({0});
        int target = graph.size() - 1;
        while(!q.empty()) {
            vector<int> path = q.front();
            q.pop();
            int cur = path.back();
            if(cur == target) {
                res.emplace_back(path);
                continue;
            }
            for(int node : graph[cur]) {
                path.emplace_back(node);
                q.push(path);
                path.pop_back();
            }
        }
        return res;
    }
    // DFS
    // vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
    //     path.clear();
    //     res.clear();
    //     path.emplace_back(0);
    //     dfs(graph, 0, graph.size() - 1);
    //     return res;
    // }
    // void dfs(vector<vector<int>>& graph, int cur_node, int target) {
    //     if(cur_node == target) {
    //         res.emplace_back(path);
    //         return;
    //     }
    //     for(int i = 0; i < graph[cur_node].size(); ++i) {
    //         path.emplace_back(graph[cur_node][i]);
    //         dfs(graph, graph[cur_node][i], target);
    //         path.pop_back();
    //     }
    // }
// private:
//     vector<int> path;
//     vector<vector<int>> res;
};
Time Complexity: O(2^n * n) 
Space Complexity: O(2^n *n ) O(n) for dfs
```
 
卡码网KamaCoder
98. 可达路径
```c++
#include<iostream>
#include<vector>
using namespace std;

vector<int> path;
vector<vector<int>> res;

void dfs(const vector<vector<int>>& graph, int cur, int target) {
    if(cur == target) {
        res.emplace_back(path);
        return;
    }
    for(int i = 1; i <= graph[cur].size(); ++i) {
        if(graph[cur][i] == 1) {
            path.emplace_back(i);
            dfs(graph, i, target);
            path.pop_back();
        }
    }
}


int main() {
    int n, m, s, t;
    cin >> n >> m;
    vector<vector<int>> graph(n + 1, vector<int>(n + 1, 0));
    while(m--) {
        cin >> s >> t;
        graph[s][t] = 1;
    }
    path.emplace_back(1);
    dfs(graph, 1, n);
    if (res.size() == 0) cout << -1 << endl;
    for (const vector<int> &pa : res) {
        for (int i = 0; i < pa.size() - 1; i++) {
            cout << pa[i] << " ";
        }
        cout << pa[pa.size() - 1]  << endl;
    }
    return 0;
}
Time Complexity: O(n) 
Space Complexity: O(n)
```
