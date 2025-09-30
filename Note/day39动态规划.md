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

## Part.11
1143. Longest Common Subsequence
```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        // 2d dp time: O(m*n) space: O(m*n)
        int m = text1.size(), n = text2.size();
        vector<vector<int>> dp (m + 1, vector<int>(n + 1, 0));
        for(int i = 1; i <= m; ++i) {
            for(int j = 1; j <= n; ++j) {
                if(text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
Time Complexity: O(m*n) 
Space Complexity: O(m*n)
```
 

1035. Uncrossed Lines
```c++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        vector<vector<int>> dp (m + 1, vector<int>(n + 1, 0));
        for(int i = 1; i <= m; ++i) {
            for(int j = 1; j <= n; ++j) {
                if(nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
Time Complexity: O(m*n) 
Space Complexity: O(m*n)
```


53. Maximum Subarray
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
    // greedy
    //     int current_sum = nums[0];
    //     int global_sum = nums[0];
    //     for(int i = 1; i < nums.size(); ++i) {
    //         current_sum = max(nums[i], current_sum + nums[i]);
    //         global_sum = max(current_sum, global_sum);
    //     }
    //     return global_sum;
    // }

    // dp
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        int res = dp[0];
        for(int i = 1; i < nums.size(); ++i) {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]);
            res = max(dp[i], res);
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1) 
```

392. Is Subsequence
```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        // dp
        int m = s.size(), n = t.size();
        vector<vector<int>> dp (m + 1, vector<int>(n + 1, 0));
        for(int i = 1; i <= m; ++i) {
            for(int j = 1; j <= n; ++j) {
                if(s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }
        if(dp[m][n] == s.size()) return true;
        return false;        
    }
};
Time Complexity: O(m*n) 
Space Complexity: O(m*n) 
```