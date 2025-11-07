# Graph
## Theory
###  并查集理论基础 Disjoint Set Union (DSU)
当我们需要判断两个元素是否在同一个集合里的时候，我们就要想到用并查集。

并查集主要有两个功能：

将两个元素添加到一个集合中。
判断两个元素在不在同一个集合
template
```c++
int n = 1005; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, 0); // C++里的一种数组结构

// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}
// 并查集里寻根的过程
int find(int u) {
    return u == father[u] ? u : father[u] = find(father[u]); // 路径压缩(path conpression)
}

// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

// 将v->u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根
    if (u == v) return ; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    father[v] = u;
}
```

## Part.5
1971. Find if Path Exists in Graph
```c++
class Solution {
public:
    void init(int n) {
        father.resize(n, 0);
        for(int i = 0; i < n; ++i) {
            father[i] = i;
        }
    }
    int find(int u) {
        if(u == father[u]) return u;
        father[u] = find(father[u]);
        return father[u];
    }
    bool isUnion(int u, int v) {
        return find(u) == find(v);
    }
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if(u == v) return;
        father[u] = v;
    }
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        init(n);
        for(vector<int>& edge : edges) {
            join(edge[0], edge[1]);
        }
        if(isUnion(source, destination)) return true;
        return false;
    }
private:
    vector<int> father;
};
Time Complexity: O(V + E)
Space Complexity: O(V)
```

 
卡码网KamaCoder
108. 多余的边
```c++
#include <iostream>
#include <vector>
using namespace std;

int n;
vector<int> father = vector<int>(101, 0);

void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}

int find(int u) {
    if(u == father[u]) return u;
    father[u] = find(father[u]);
    return father[u];
}

bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    if(u == v) return true;
    return false;
}

void join(int u, int v) {
    u = find(u);
    v = find(v);
    if(u == v) return;
    father[u] = v;
}

int main() {
    int m, s, t, source, destination;
    cin >> n >> m;
    init();
    while(m--) {
        cin >> s >> t;
        join(s,t);
    }
    cin >> source >> destination;
    if(isSame(source, destination)) {
        cout << 1 << endl;
    } else {
        cout << 0 << endl;
    }
    return 0;
}
Time Complexity: O(m * n)
Space Complexity: O(m * n)
```
684. Redundant Connection
```c++
class Solution {
public:
    void init(int n) {
        parent.resize(n + 1);
        for(int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    int find(int u) {
        if(u == parent[u]) return u;
        parent[u] = find(parent[u]);
        return parent[u];
    }
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if(u == v) return;
        parent[u] = v;
    }
    bool isUnion(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        init(n);
        for(vector<int>& edge : edges) {
            if(isUnion(edge[0], edge[1])) {
                return edge;
            } else {
                join(edge[0], edge[1]);
            }
        }
        return {};
    }
private:
    vector<int> parent;
};
Time Complexity: O(n) 
Space Complexity: O(n)
```

卡码网KamaCoder
109. 多余的边II
```c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> father;

void init(int n) {
    father.resize(n + 1);
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}

int find(int u) {
    if(u == father[u]) return u;
    father[u] = find(father[u]);
    return father[u];
}

bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    if(u == v) return true;
    return false;
}

void join(int u, int v) {
    u = find(u);
    v = find(v);
    if(u == v) return;
    father[v] = u;
}

int main() {
    int n, s, t;
    vector<vector<int>> edges;
    cin >> n; 
    for (int i = 0; i < n; i++) {
        cin >> s >> t;
        edges.push_back({s, t});
    }
    vector<int> indegree(n + 1, 0);
    vector<int> firstEdge, secondEdge;
    for(auto& edge : edges) {
        int u = edge[0], v = edge[1];
        if(indegree[v] == 0) {
            indegree[v] = u;
        } else {
            firstEdge = {indegree[v], v};
            secondEdge = {u, v};
            edge[1] = 0;
        }
    }
    init(n);
    for(auto& edge : edges) {
        int u = edge[0], v = edge[1];
        if(v == 0) continue;
        if(isSame(u, v)) {
            if(firstEdge.empty()) { // Cycle, no two parents, remove current edge
                cout << edge[0] << " " << edge[1] << endl;
                return 0;
            } else {
                cout << firstEdge[0] << " " << firstEdge[1] << endl;
                return 0; // Cycle, two parents, remove first edge
            }
        } else {
            join(u, v);
        }
    }
    cout << secondEdge[0] << " " << secondEdge[1] << endl;
    return 0; // no cycle, two parents, remove second edge
}
Time Complexity: O(n) 
Space Complexity: O(n)
```

685. Redundant Connection II
```c++
class Solution {
public:
    void init(int n) {
        parent.resize(n + 1);
        for(int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    int find(int u) {
        if(u == parent[u]) return u;
        parent[u] = find(parent[u]);
        return parent[u];
    }
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if(u == v) return;
        parent[v] = u;
    }
    bool isUnion(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> indegree(n + 1, 0);
        vector<int> firstEdge, secondEdge;
        for(auto& edge : edges) {
            int u = edge[0], v = edge[1];
            if(indegree[v] == 0) {
                indegree[v] = u;
            } else {
                firstEdge = {indegree[v], v};
                secondEdge = {u, v};
                edge[1] = 0;
            }
        }

        init(n);
        for(auto& edge : edges) {
            int u = edge[0], v = edge[1];
            if(v == 0) continue;
            if(isUnion(u, v)) {
                if(firstEdge.empty()) { // Cycle, no two parents, remove current edge
                    return edge;
                } else {
                    return firstEdge; // Cycle, two parents, remove first edge
                }
            } else {
                join(u, v);
            }
        }
        return secondEdge; // no cycle, two parents, remove second edge
    }
private:
    vector<int> parent;
};
Time Complexity: O(n) 
Space Complexity: O(n)
```