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

## Part.8
121. Best Time to Buy and Sell Stock
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // greedy
        // int buy_price = prices[0];
        // int maxProfit = 0;
        // for(int i = 1; i < prices.size(); ++i) {
        //     buy_price = min(buy_price, prices[i]);
        //     maxProfit = max(maxProfit, prices[i] - buy_price);
        // }
        // return maxProfit;

        // 2d dp
//         int len = prices.size();
//         vector<vector<int>> dp(len, vector<int>(2, 0));
//         // hold
//         dp[0][0] = -prices[0];
//         // not hold 
//         dp[0][1] = 0;
//         for(int i = 1; i < len; ++i) {
//             dp[i][0] = max(dp[i - 1][0], -prices[i]);
//             dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
//         }
//         return dp[len - 1][1];
//     }
        // 1d dp
        int len = prices.size();
        int hold = -prices[0];
        int notHold = 0;
        for(int i = 1; i < len; ++i) {
            notHold = max(notHold, prices[i] + hold);
            hold = max(hold, -prices[i]);
        }
        return notHold;
    }
};
Time Complexity: O(n)
Space Complexity: O(1)
```
 

122. Best Time to Buy and Sell Stock II
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // greedy
    //     int profit = 0;
    //     for(int i = 1; i < prices.size(); ++i) {
    //         if(prices[i] - prices[i - 1] > 0) {
    //             profit += prices[i] - prices[i - 1];
    //         }
    //     }
    //     return profit;
    // }

        // 2d dp
    //     int len = prices.size();
    //     vector<vector<int>> dp(len, vector<int>(2, 0));
    //     dp[0][0] = -prices[0];
    //     dp[0][1] = 0;
    //     for(int i = 1; i < len; ++i) {
    //         dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
    //         dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
    //     }
    //     return dp[len - 1][1];
    // }

        // 1d dp
        int len = prices.size();
        int hold = -prices[0];
        int notHold = 0;
        for(int i = 1; i < len; ++i) {
            int prevNotHold = notHold;
            notHold = max(notHold, hold + prices[i]);
            hold = max(hold, prevNotHold - prices[i]);
        }
        return notHold;
    }
};
Time Complexity: O(n)
Space Complexity: O(1)
```


123. Best Time to Buy and Sell Stock III
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy1 = INT_MIN, sell1 = 0;
        int buy2 = INT_MIN, sell2 = 0;
        for(int& cur_price : prices) {
            buy1 = max(buy1, - cur_price);
            sell1 = max(sell1, cur_price + buy1);
            buy2 = max(buy2, sell1 - cur_price);
            sell2 = max(sell2, cur_price + buy2);
        }
        return sell2;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1) 
```