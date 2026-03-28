# Graph
## Theory
###  最短路算法之Dijkstra
dijkstra算法：在有权图（权值非负数）中求从起点到其他节点的最短路径算法。

需要注意两点：

dijkstra 算法可以同时求 起点到所有节点的最短路径
权值不能为负数

这里我也给出 dijkstra三部曲：

第一步，选源点到哪个节点近且该节点未被访问过
第二步，该最近节点被标记访问过
第三步，更新非访问节点到源点的距离（即更新minDist数组）
## Part.9
卡码网KamaCoder
47. 参加科学大会
```c++
// #include<iostream>
// #include<vector>
// #include<climits>
// using namespace std;

// int main() {
//     int n, m, s, e, val;
//     cin >> n >> m;
//     vector<vector<int>> grid(n + 1, vector<int>(n + 1, INT_MAX));
//     while(m--) {
//         cin >> s >> e >> val;
//         grid[s][e] = val;
//     }
//     vector<bool> visited(n + 1, false);
//     vector<int> minDis(n + 1, INT_MAX);
//     int start = 1;
//     minDis[start] = 0;
//     for(int i = 1; i <= n; ++i) {
//         int cur = 0;
//         int minVal = INT_MAX;
//         for(int j = 1; j <= n; ++j) {
//             if(!visited[j] && minDis[j] < minVal) {
//                 minVal = minDis[j];
//                 cur = j;
//             }
//         }
//         visited[cur] = true;
//         for(int j = 1; j <= n; ++j) {
//             if(!visited[j] && grid[cur][j] != INT_MAX && minDis[cur] + grid[cur][j] < minDis[j]) {
//                 minDis[j] = minDis[cur] + grid[cur][j];
//             }
//         }
//     }
//     if(minDis[n] == INT_MAX) {
//         cout << "-1" << endl;
//     } else {
//         cout << minDis[n] << endl;
//     }
// }
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include<iostream>
#include<vector>
#include<list>
#include <queue>
#include<climits>
using namespace std;

class mycomparison {
public:
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        return lhs.second > rhs.second;
    }
};
struct Edge {
    int t;
    int val;
    Edge(int t, int val) : t(t), val(val) {};
};
int main() {
    int n, m, s, e, val;
    cin >> n >> m;
    vector<list<Edge>> grid(n + 1);
    while(m--) {
        cin >> s >> e >> val;
        grid[s].push_back(Edge(e, val));
    }
    vector<bool> visited(n + 1, false);
    vector<int> minDis(n + 1, INT_MAX);
    int start = 1;
    minDis[start] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> q;
    q.push({start, 0});
    while(!q.empty()) {
        pair<int, int> cur = q.top();
        q.pop();
        if(visited[cur.first]) continue;
        visited[cur.first] = true;
        for(Edge edge : grid[cur.first]) {
            if(!visited[edge.t] && minDis[cur.first] + edge.val < minDis[edge.t]) {
                minDis[edge.t] = minDis[cur.first] + edge.val;
                q.push({edge.t, minDis[edge.t]});
            }
        }
    }
    if(minDis[n] == INT_MAX) {
        cout << "-1" << endl;
    } else {
        cout << minDis[n] << endl;
    }
}
Time Complexity: O(ElogE)
Space Complexity: O(E + V)
```

743. Network Delay Time
```c++
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<list<pair<int,int>>> graph(n + 1);
        for(auto& time : times) {
            graph[time[0]].push_back({time[1], time[2]});
        }
        vector<bool> visited(n + 1, false);
        vector<int> minDis(n + 1, INT_MAX);
        minDis[k] = 0;
        auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {
            return a.second > b.second;
        };
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> q(cmp);
        q.push({k, 0});
        while(!q.empty()) {
            pair<int, int> cur = q.top();
            q.pop();
            if(visited[cur.first]) continue;
            visited[cur.first] = true;
            for(auto& edge : graph[cur.first]) {
                if(!visited[edge.first] && minDis[cur.first] + edge.second < minDis[edge.first]) {
                    minDis[edge.first] = minDis[cur.first] + edge.second;
                    q.push({edge.first, minDis[edge.first]});
                }
            }
        }
        int res = 0;
        for(int i = 1; i <= n; ++i) {
            if(minDis[i] == INT_MAX) return -1;
            res = max(minDis[i], res);
        }
        return res;
    }
};
Time Complexity: O(ElogE)
Space Complexity: O(e + v)
```

210. Course Schedule II
```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        for(auto& p : prerequisites) {
            int a = p[0];
            int b = p[1];
            graph[b].emplace_back(a);
            indegree[a]++;
        }
        queue<int> q;
        vector<int> res;
        for(int i = 0; i < numCourses; ++i) {
            if(indegree[i] == 0) {
                res.emplace_back(i);
                q.push(i);
            }
        }
        while(!q.empty()) {
            int cur = q.front();
            q.pop();
            for(int next : graph[cur]) {
                indegree[next]--;
                if(indegree[next] == 0) {
                    res.emplace_back(next);
                    q.push(next);
                }
            }
        }
        if(res.size() != numCourses) return {};
        return res;
    }
};
Time Complexity: O(e + v)
Space Complexity: O(e + v)
```
