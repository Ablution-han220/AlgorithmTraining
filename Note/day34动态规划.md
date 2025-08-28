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

## Part.6
322. Coin Change
```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int max = amount + 1;
        vector<int> dp(amount + 1, max);
        dp[0] = 0;
        for(int i = 0; i <= amount; ++i) {
            for(int j = 0; j < coins.size(); ++j) {
                if(i >= coins[j]) {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1); 
                }
            }
        }
        if(dp[amount] == max) {
            return -1;
        }
        return dp[amount];
    }
};
Time Complexity: O(n*amount)
Space Complexity: O(amount)
```
 

279. Perfect Squares
```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        // outer loop for item, inner loop for bag
        // for(int i = 1; i*i <= n; ++i) {
        //     for(int j = i*i; j <= n; ++j) {
        //         dp[j] = min(dp[j], dp[j - i*i] + 1);
        //     }
        // }
        // outer loop for bag, inner loop for item
        for(int i = 0; i <= n; ++i) {
            for(int j = 1; j*j <= i; ++j) {
                dp[i] = min(dp[i], dp[i - j*j] + 1);
            }
        }
        return dp[n];
    }
};
Time Complexity: O(n * sqrt(n))
Space Complexity: O(n)
```

139. Word Break
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        int n = s.size();
        vector<bool> dp(n + 1, false);
        dp[0] = true;
        for(int i = 1; i <= n; ++i) {
            for(int j = 0; j < i; ++j) {
                if(dp[j] == true && dict.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
};
Time Complexity: O(n^3) worst case because s.substr allocates a new string and copies characters, which costs O(len) = O(i-j) in the worst case.
Space Complexity: O(n)
```