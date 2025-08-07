# Greedy Algorithm
## Theory
本质是选择局部最优从而达到全局最优
贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
2. 找出适合的贪心策略
3. 求解每一个子问题的最优解
4. 将局部最优解堆叠成全局最优解

## Part.5
56. Merge Intervals
```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> res;
        res.emplace_back(intervals[0]);
        for(int i = 1; i < intervals.size(); ++i) {
            if(res.back()[1] >= intervals[i][0]) {
                res.back()[1] = max(res.back()[1], intervals[i][1]);
            } else {
                res.emplace_back(intervals[i]);
            }
        }
        return res;
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(n)
```

738. Monotone Increasing Digits
```c++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string s = to_string(n);
        int mark = s.size();
        for(int i = s.size() - 1; i > 0; --i) {
            if(s[i - 1] > s[i]) {
                s[i - 1]--;
                mark = i;
            }
        }
        for(int i = mark; i < s.size(); ++i) {
            s[i] = '9';
        }
        return stoi(s);
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```


968. Binary Tree Cameras
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    //We classify each node into three states:
    // 0 → Node needs a camera (not covered by its children or parent yet)
    // 1 → Node has a camera
    // 2 → Node is covered (by its children or parent’s camera) but doesn’t have one
    int traveral(TreeNode* cur) {
        // Null nodes are considered covered
        if(cur == nullptr) return 2;
        int left = traveral(cur->left);
        int right = traveral(cur->right);
        // child needs camera
        if(left == 0 || right == 0) {
            res++;
            return 1; // place a camera
        }
        // child has camera
        if(left == 1 || right == 1) {
            return 2; // covered
        }
        // child covered but no camera → needs camera
        return 0;
    }
    int minCameraCover(TreeNode* root) {
        res = 0;
        if(traveral(root) == 0) res++; // root still needs camera
        return res;
    }
private:
    int res;
};
Time Complexity: O(n) 
Space Complexity: O(n)
```
Note:  
greedy algorithm to place as many as camera on leaf's parents node. Then, classify each node into three states
