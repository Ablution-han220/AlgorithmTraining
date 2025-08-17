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

## Part.4 
1049. Last Stone Weight II
```c++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        for(int& weight : stones) {
            sum += weight;
        }
        int target = sum / 2;
        vector<int> dp(target + 1, 0);
        for(int i = 0; i < stones.size(); ++i) {
            for(int j = target; j >= stones[i]; --j) {
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - dp[target] - dp[target];
    }
};
Time Complexity: O(n*target)
Space Complexity: O(target)
```
494. Target Sum
```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for(int n : nums) sum += n;
        if(abs(target) > sum) return 0;
        if((sum - target) % 2 == 1) return 0;
        int bagSize = (sum - target) / 2;
        vector<int> dp(bagSize + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < nums.size(); ++i) {
            for(int j = bagSize; j >= nums[i]; --j) {
                dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
};
Time Complexity: O(n*bagSize) where bagSize = (sum - target) / 2
Space Complexity: O(bagSize)
```

474. Ones and Zeroes
```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for(string& s : strs) {
            int zeroNum = 0, oneNum = 0;
            for(char c : s) {
                if(c == '0') zeroNum++;
                else oneNum++;
            }
            for(int i = m; i >= zeroNum; --i) {
                for(int j = n; j >= oneNum; --j) {
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
Time Complexity: O(kmn) where k = strs.size()
Space Complexity: O(mn)
```