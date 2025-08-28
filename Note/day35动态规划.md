# Dynamic Programming
## Theory
如果某一问题有很多重叠子问题，使用动态规划是最有效的。

所以动态规划中每一个状态一定是由上一个状态推导出来的，这一点就区分于贪心，贪心没有状态推导，而是从局部直接选最优的，

状态转移公式（递推公式）是很重要，但动规不仅仅只有递推公式。

对于动态规划问题，我将拆解为如下五步曲，这五步都搞清楚了，才能说把动态规划真的掌握了！

1.组（dp table）以及下标的含义
2.递推公式
3.数组如何初始化
4.遍历顺序
5.推导dp数组

## Part.7
198. House Robber
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i = 2; i < nums.size(); ++i) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.size() - 1];
    }
};
Time Complexity: O(n)
Space Complexity: O(n)
```
 

213. House Robber II
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1) return nums[0];
        if(nums.size() == 2) return max(nums[0], nums[1]);
        int res1 = robRange(nums, 0, nums.size() - 2);
        int res2 = robRange(nums, 1, nums.size() - 1);
        return max(res1, res2);
    }
private:
    int robRange(vector<int>& nums, int left, int right) {
        int dp0 = nums[left];
        int dp1 = max(nums[left], nums[left + 1]);
        for(int i = left + 2; i <= right; ++i) {
            int cur = max(dp0 + nums[i], dp1);
            dp0 = dp1;
            dp1 = cur;
        }
        return dp1;
    }
};
Time Complexity: O(n)
Space Complexity: O(1)
```
Note:  
for cicrle problem, try to split into linear problem

337. House Robber III
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
    int rob(TreeNode* root) {
        vector<int> res = robTree(root);
        return max(res[0], res[1]);
    }
    vector<int> robTree(TreeNode* cur) {
        if(cur == nullptr) return vector<int>{0, 0};
        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        // rob
        int val1 = cur->val + left[0] + right[0];
        // not rob
        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2, val1};
    }
};
Time Complexity: O(n) 
Space Complexity: O(logn) 
```