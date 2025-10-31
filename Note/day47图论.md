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

## Part.4
127. Word Ladder
```c++
class Solution {
public:
    // bidirectional BFS
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        if(!dict.count(endWord)) return 0;
        unordered_set<string> beginSet{beginWord};
        unordered_set<string> endSet{endWord};
        unordered_set<string> visited;
        int path = 1;
        while(!beginSet.empty() && !endSet.empty()) {
            // Always expand smaller frontier
            if(beginSet.size() > endSet.size()) swap(beginSet, endSet);
            unordered_set<string> next;
            for(const string& word : beginSet) {
                string cur = word;
                for(int i = 0; i < cur.size(); ++i) {
                    char origin = cur[i];
                    for(char c = 'a'; c <= 'z'; ++c) {
                        if(c == origin) continue;
                        cur[i] = c;
                        if(endSet.count(cur)) return path + 1;
                        if(dict.count(cur) && !visited.count(cur)) {
                            visited.insert(cur);
                            next.insert(cur);
                        }
                    }
                    cur[i] = origin;
                }
            }
            beginSet = move(next);
            ++path;
        }
        return 0;
    }
    // single bfs
    // int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    //     unordered_set<string> dict(wordList.begin(), wordList.end());
    //     if(!dict.count(endWord)) return 0;
    //     int n = beginWord.size();
    //     unordered_map<string, vector<string>> combo;
    //     for(string word : dict) {
    //         for(int i = 0; i < n; ++i) {
    //             string pattern = word.substr(0, i) + "*" + word.substr(i + 1, n - 1);
    //             combo[pattern].emplace_back(word);
    //         }
    //     }
    //     queue<pair<string, int>> q;
    //     q.push({beginWord, 1});
    //     unordered_set<string> visited;
    //     visited.insert(beginWord);
    //     while(!q.empty()) {
    //         pair<string, int> cur = q.front();
    //         q.pop();
    //         string word = cur.first;
    //         int path = cur.second;
    //         for(int i = 0; i < n; ++i) {
    //             string pattern = word.substr(0, i) + "*" + word.substr(i + 1, n - 1);
    //             for(string next : combo[pattern]) {
    //                 if(next == endWord) {
    //                     return path + 1;
    //                 }
    //                 if(!visited.count(next)) {
    //                     visited.insert(next);
    //                     q.push({next, path + 1});
    //                 }
    //             }
    //         }
    //     }
    //     return 0;
    // }
};
Time Complexity: O(N × L²) where N = number of words, L = word length
                O(N × L) rougly when using bidirectional BFS 
Space Complexity: O(N × L)
```

 
卡码网KamaCoder
110. 字符串迁移
```c++
#include <string>
#include <unordered_set>
#include <unordered_map>
#include <queue>
#include <vector>
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    string beginStr, endStr, str;
    unordered_set<string> dict;
    cin >> beginStr >> endStr;
    for(int i = 0; i < n; ++i) {
        cin >> str;
        dict.insert(str);
    }
    unordered_set<string> beginSet{beginStr};
    unordered_set<string> endSet{endStr};
    unordered_set<string> visited;
    int path = 1;
    while(!beginSet.empty() && !endSet.empty()) {
        if(beginSet.size() > endSet.size()) swap(beginSet, endSet);
        unordered_set<string> next;
        for(const string& word : beginSet) {
            string cur = word;
            for(int i = 0; i < cur.size(); ++i) {
                char origin = cur[i];
                for(char c = 'a'; c <= 'z'; ++c) {
                    if(c == cur[i]) continue;
                    cur[i] = c;
                    if(endSet.count(cur)) {
                        cout << path + 1 << endl;
                        return 0;
                    }
                    if(dict.count(cur) && !visited.count(cur)) {
                        visited.insert(cur);
                        next.insert(cur);
                    }
                }
                cur[i] = origin;
            }
        }
        beginSet = next;
        path++;
    }
    cout << 0 << endl;
    return 0;
}
Time Complexity: O(N × L) where N = number of words, L = word length
Space Complexity: O(N × L)
```

卡码网KamaCoder
105. 有向图的完全联通
```c++
#include<iostream>
#include<vector>
#include<list>
using namespace std;

void dfs(const vector<list<int>>& graph, vector<bool>& visited, int key) {
    if(visited[key]) return;
    visited[key] = true;
    const list<int>& keys = graph[key];
    for(const int& k : keys) {
        dfs(graph, visited, k);
    }
}

int main() {
    int n, k, s, t;
    cin >> n >> k;
    vector<list<int>> graph(n + 1);
    while(k--) {
        cin >> s >> t;
        graph[s].push_back(t);
    }
    vector<bool> visited(n + 1, false);
    dfs(graph, visited, 1);
    for(int i = 1; i <= n; ++i) {
        if(visited[i] == false) {
            cout << -1 << endl;
            return 0;
        }
    }
    cout << 1 << endl;
    return 0;
}
Time Complexity: O(N + k) where N = number of nodes, k = number of directed edges
Space Complexity: O(N + k)
```

卡码网KamaCoder
106. 海岸线计算
```c++
#include<iostream>
#include<vector>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            cin >> grid[i][j];
        }
    }
    int res = 0;
    int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j) {
            if(grid[i][j] == 1) {
                for(int k = 0; k < 4; ++k) {
                    int x = i + dir[k][0];
                    int y = j + dir[k][1];
                    if(x < 0 || x >= n || y < 0 || y >= m || grid[x][y] == 0) {
                        res++;
                    }             
                }
            }
        }
    }
    cout << res << endl;
    return 0;
}
Time Complexity: O(m * n) 
Space Complexity: O(m * n)
```

463. Island Perimeter
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        int res = 0;
        int dir[4][2] = {{0,1}, {1, 0}, {0, -1}, {-1,0}};
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(grid[i][j] == 1) {
                    for(int k = 0; k < 4; ++k) {
                        int x = i + dir[k][0];
                        int y = j + dir[k][1];
                        if(x < 0 || x >= n || y < 0 || y >= m || grid[x][y] == 0) {
                            res++;
                        }
                    }
                }
            }
        }
        return res;
    }
};
Time Complexity: O(m * n) 
Space Complexity: O(m * n)
```