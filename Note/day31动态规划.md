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

## 0/1 knapsack problem
1. dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。

2. 推理递归公式：  
不放物品i：背包容量为j，里面不放物品i的最大价值是dp[i - 1][j]。

放物品i：背包空出物品i的容量后，背包容量为j - weight[i]，dp[i - 1][j - weight[i]] 为背包容量为j - weight[i]且不放物品i的最大价值，那么dp[i - 1][j - weight[i]] + value[i] （物品i的价值），就是背包放物品i得到的最大价值

递归公式： dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

3. 初始化

## Part.3 
416. Partition Equal Subset Sum
```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        // dp  time:O(m*n)  space:O(m) where mm is the subSetSum, and nn is the number of array elements
        int n = nums.size();
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if (sum % 2 != 0) {
            return false;
        }
        int subSum = sum / 2;
        vector<bool> dp(subSum + 1, false);
        dp[0] = true;
        for (int i = 0; i < n; ++i) {
            int cur = nums[i];
            for (int j = subSum; j >= cur; --j) {
                dp[j] = dp[j] || dp[j - cur];
            }
        }
        return dp[subSum];
    }
};
Time Complexity: O(n*target)
Space Complexity: O(target)
```