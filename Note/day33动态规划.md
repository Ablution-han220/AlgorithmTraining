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

## unbounded knapsack problem (BKP)
1. dp[i][j] 表示从下标为[0-i]的物品，每个物品可以取无限次，放进容量为j的背包，价值总和最大是多少。

2. 推理递归公式：  
不放物品i：背包容量为j，里面不放物品i的最大价值是dp[i - 1][j]。

放物品i：背包空出物品i的容量后，背包容量为j - weight[i]，dp[i][j - weight[i]] 为背包容量为j - weight[i]且不放物品i的最大价值，那么dp[i][j - weight[i]] + value[i] （物品i的价值），就是背包放物品i得到的最大价值

递归公式： dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);

（注意，完全背包二维dp数组 和 01背包二维dp数组 递推公式的区别，01背包中是 dp[i - 1][j - weight[i]] + value[i]);

3. 初始化

4. 遍历顺序
在求装满背包有几种方案的时候，认清遍历顺序是非常关键的。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

## Part.5
518. Coin Change II
```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < coins.size(); ++i) {
            for(int j = coins[i]; j <= amount; ++j) {
                if(dp[j] < INT_MAX - dp[j - coins[i]]) {
                    dp[j] = dp[j] + dp[j - coins[i]];
                }
            }
        }
        return dp[amount];
    }
};
Time Complexity: O(n*amount)
Space Complexity: O(amount)
```
Note:  
overflow concern,  care Iteration order
 

377. Combination Sum IV
```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for(int i = 0; i <= target; ++i) {
            for(int j = 0; j < nums.size(); ++j) {
                if(i - nums[j] >= 0 && dp[i] <= INT_MAX - dp[i - nums[j]]) {
                    dp[i] = dp[i] + dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
};
Time Complexity: O(n*target) 
Space Complexity: O(target)
```