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

## Part.9
188. Best Time to Buy and Sell Stock IV
```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if (n == 0) return 0;

        // Case 1: unlimited transactions
        if (k >= n / 2) {
            int profit = 0;
            for (int i = 1; i < n; i++) {
                if (prices[i] > prices[i-1]) profit += prices[i] - prices[i-1];
            }
            return profit;
        }

        // Case 2: DP
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(k+1, vector<int>(2, 0)));

        for (int j = 0; j <= k; j++) {
            dp[0][j][0] = 0;
            dp[0][j][1] = -prices[0];
        }

        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= k; j++) {
                dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i]);
                dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i]);
            }
        }

        return dp[n-1][k][0];
    }
};
Time Complexity: O(n*k)
Space Complexity: O(n*k)
```
 

309. Best Time to Buy and Sell Stock with Cooldown
```c++

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int hold = -prices[0];
        int sold = 0;
        int rest = 0;
        for(int i = 1; i < n; ++i) {
            int preHold = hold;
            int preSold = sold;
            int preRest = rest;
            hold = max(preHold, preRest - prices[i]);
            sold = preHold + prices[i];
            rest = max(preRest, preSold);
        }
        return max(sold, rest);
    }
};
Time Complexity: O(n)
Space Complexity: O(1)
```


714. Best Time to Buy and Sell Stock with Transaction Fee
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int len = prices.size();
        int hold = -prices[0];
        int notHold = 0;
        for(int i = 1; i < len; ++i) {
            int prevNotHold = notHold;
            notHold = max(notHold, hold + prices[i] - fee);
            hold = max(hold, prevNotHold - prices[i]);
        }
        return notHold;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1) 
```