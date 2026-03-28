# Graph
## Theory
###  拓扑排序精讲(Topological sorting)
给出一个 有向图，把这个有向图转成线性的排序 就叫拓扑排序。   
当然拓扑排序也要检测这个有向图 是否有环，即存在循环依赖的情况，因为这种情况是不能做线性排序的。  
所以拓扑排序也是图论中判断有向无环图的常用方法。  

拓扑排序的过程，其实就两步：

找到入度为0 的节点，加入结果集
将该节点从图中移除
## Part.8
卡码网KamaCoder
117. 软件构建
```c++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int m, n, s, t;
    cin >> n >> m;
    vector<int> indegree(n, 0);
    vector<vector<int>> graph(n);
    while(m--) {
        cin >> s >> t;
        indegree[t]++;
        graph[s].emplace_back(t);
    }
    queue<int> q;
    vector<int> res;
    for(int i = 0; i < n; ++i) {
        if(indegree[i] == 0) {
            q.push(i);
        }
    }
    while(!q.empty()) {
        int cur = q.front();
        q.pop();
        res.emplace_back(cur);
        for(int next : graph[cur]) {
            indegree[next]--;
            if(indegree[next] == 0) {
                q.push(next);
            }
        }
    }
    if(res.size() != n) {
        cout << "-1" << endl;
    } else {
        for(int i = 0; i < n - 1; ++i) {
            cout << res[i] << " ";
        }
        cout << res[n - 1];
    }

}
Time Complexity: O(e + v)
Space Complexity: O(e + v)
```

207. Course Schedule
```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        for(auto& p : prerequisites) {
            int a = p[0];
            int b = p[1];
            graph[b].emplace_back(a);
            indegree[a]++;
        }
        queue<int> q;
        for(int i = 0; i < numCourses; ++i) {
            if(indegree[i] == 0) {
                q.push(i);
            }
        }
        int count = 0;
        while(!q.empty()) {
            int cur = q.front();
            q.pop();
            count++;
            for(int next : graph[cur]) {
                indegree[next]--;
                if(indegree[next] == 0) {
                    q.push(next);
                }
            }
        }
        return count == numCourses;
    }
};
Time Complexity: O(e + v)
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
