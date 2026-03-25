# Graph
## Theory
###  最小生成树之Minimum spanning tree（kruscal算法）
prim 算法是维护节点的集合，而 Kruskal 是维护边的集合。  
kruscal的思路：

1.边的权值排序，因为要优先选最小的边加入到生成树里   
2.遍历排序后的边      
如果边首尾的两个节点在同一个集合，说明如果连上这条边图中会出现环  
如果边首尾的两个节点不在同一个集合，加入到最小生成树，并把两个节点加入同一个集合 

## Part.7
卡码网KamaCoder
53. 寻宝
```c++
#include<iostream>
#include<vector>
#include <algorithm>
using namespace std;

struct Edges{
    int v1, v2, val;
};

int n = 10001; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, -1); // C++里的一种数组结构

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

int main() {
    int v, e;
    int v1, v2, val;
    cin >> v >> e;
    vector<Edges> edges;
    while(e--) {
        cin >> v1 >> v2 >> val;
        edges.push_back({v1, v2, val});

    }
    sort(edges.begin(), edges.end(), [](const Edges &a, const Edges &b ){
        return a.val < b.val;
    });
    init();
    int res = 0;
    for(Edges edge: edges) {
        if(!isSame(edge.v1, edge.v2)) {
            join(edge.v1, edge.v2);
            res+= edge.val;
        }
    }
    cout << res << endl;
}
Time Complexity: O(eloge)
Space Complexity: O(e + v)
```
