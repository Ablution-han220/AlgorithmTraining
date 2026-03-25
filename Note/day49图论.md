# Graph
## Theory
###  最小生成树之Minimum spanning tree（Prim算法）
prim算法是从节点的角度采用贪心的策略每次寻找距离最小生成树最近的节点并加入到最小生成树中。

prim算法核心就是三步，我称为prim三部曲，大家一定要熟悉这三步，代码相对会好些很多：

第一步，选距离生成树最近节点
第二步，最近节点加入生成树
第三步，更新非生成树节点到生成树的距离（即更新minDist数组）

## Part.6
卡码网KamaCoder
53. 寻宝
```c++
#include<iostream>
#include<vector>
#include <climits>
using namespace std;

int main() {
    int v, e;
    int v1, v2, val;
    cin >> v >> e;
    vector<vector<int>> grid(v + 1, vector<int>(v + 1, 10001));
    while(e--) {
        cin >> v1 >> v2 >> val;
        grid[v1][v2] = val;
        grid[v2][v1] = val;
    }
    vector<bool> visited(v + 1, false);
    vector<int> minDis(v + 1, 10001);
    for(int i = 1; i < v; ++i) {
        int cur = -1;
        int minVal = INT_MAX;
        for(int j = 1; j <= v; ++j) {
            if(minDis[j] < minVal && !visited[j]) {
                minVal = minDis[j];
                cur = j;
            }
        }
        visited[cur] = true;
        for(int j = 1; j <= v; ++j) {
            if(!visited[j] && grid[cur][j] < minDis[j]) {
                minDis[j] = grid[cur][j];
            }
        }
    }
    int res = 0;
    for(int i = 2; i <= v; ++i) {
        res += minDis[i];
    }
    cout << res << endl;
}
Time Complexity: O(v * v)
Space Complexity: O(v * v)
```

1584. Min Cost to Connect All Points
```c++
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int v = points.size();
        vector<bool> visited(v, false);
        vector<int> minDis(v, INT_MAX);
        minDis[0] = 0;
        for(int i = 0; i < v; ++i) {
            int cur = -1;
            int minVal = INT_MAX;
            for(int j = 0; j < v; ++j) {
                if(!visited[j] && minDis[j] < minVal) {
                    minVal = minDis[j];
                    cur = j;
                }
            }
            visited[cur] = true;
            for(int j = 0; j < v; ++j) {
                if(!visited[j]) {
                    int dis = abs(points[cur][0] - points[j][0]) + abs(points[cur][1] - points[j][1]);
                    minDis[j] = min(dis, minDis[j]);
                }
            }
        }
        int res = 0;
        for(int i = 0; i < v; ++i) {
            res += minDis[i];
        }
        return res;
    }
};
Time Complexity: O(v * v)
Space Complexity: O(v)
```
