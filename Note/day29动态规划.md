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

## Part.1
509. Fibonacci Number
```c++
class Solution {
public:
    int fib(int n) {
        if(n < 2) return n;
        int dp[2];
        dp[0] = 0;
        dp[1] = 1; 
        for(int i = 2; i <= n; ++i) {
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;  
        }
        return dp[1];
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```

70. Climbing Stairs
```c++
class Solution {
public:
    int climbStairs(int n) {
        if(n < 3) return n;
        int dp[3];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; ++i) {
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```

746. Min Cost Climbing Stairs
```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int dp[2];
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i <= cost.size(); ++i) {
            int dpi = min(dp[0] + cost[i - 2], dp[1] + cost[i - 1]);
            dp[0] = dp[1];
            dp[1] = dpi;
        }
        return dp[1];
    }
};
Time Complexity: O(n) 
Space Complexity: O(1
```

