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

## Part.2
62. Unique Paths
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        // 2d array
        // vector<vector<int>> dp(m, vector<int> (n,0));
        // for(int i = 0; i < m; ++i) dp[i][0] = 1;
        // for(int j = 0; j < n; ++j) dp[0][j] = 1;
        // for(int i = 1; i < m; ++i) {
        //     for(int j = 1; j < n; ++j) {
        //         dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        //     }
        // }
        // return dp[m - 1][n - 1];

        // 1d array
        vector<int> dp(n, 1);
        for(int i = 1; i < m; ++i) {
            for(int j = 1; j < n; ++j) {
                dp[j] = dp[j] + dp[j - 1];
            }
        }
        return dp[n - 1];
    }
};
Time Complexity: O(m*n)
Space Complexity: O(n) if use rolling array, else O(m*n)
```

63. Unique Paths II
```c++
class Solution {
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        // 2d array
        // int m = obstacleGrid.size();
        // int n = obstacleGrid[0].size();
        // if(obstacleGrid[0][0] == 1 || obstacleGrid[m - 1][n - 1] == 1 ) return 0;
        // vector<vector<int>> dp(m, vector<int>(n, 0));
        // for(int i = 0; i < m && obstacleGrid[i][0] == 0; ++i) dp[i][0] = 1;
        // for(int j = 0; j < n && obstacleGrid[0][j] == 0; ++j) dp[0][j] = 1;
        // for(int i = 1; i < m; ++i) {
        //     for(int j = 1; j < n; ++j) {
        //         if(obstacleGrid[i][j] == 0) {
        //             dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        //         }
        //     }
        // }
        // return dp[m - 1][n - 1];
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<int> dp(n, 0);
        dp[0] = (obstacleGrid[0][0] == 1) ? 0 : 1;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else if(j > 0) {
                    dp[j] = dp[j] + dp[j - 1];
                }
            }
        }
        return dp[n - 1];
    }
};
Time Complexity: O(m*n)
Space Complexity: O(n) if use rolling array, else O(m*n)
```

343. Integer Break
```c++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1);
        dp[2] = 1;
        for(int i = 3; i <= n; ++i) {
            for(int j = 1; j < i; ++j) {
                dp[i] = max(dp[i], max(dp[i - j] * j, (i - j) * j));
            }
        }
        return dp[n];
    }
};
Time Complexity: O(n^2)
Space Complexity: O(n)
```

96. Unique Binary Search Trees
```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= i; ++j) {
                dp[i] = dp[i] + dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
Time Complexity: O(n^2)
Space Complexity: O(n)
```
