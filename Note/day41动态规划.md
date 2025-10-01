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

## Part.13
647. Palindromic Substrings
```c++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        int res = 0;
        for(int i = n - 1; i >= 0; --i) {
            for(int j = i; j < n; ++j) {
                if(s[i] == s[j]) {
                    if(j - i <= 1) {
                        res++;
                        dp[i][j] = true;
                    } else {
                        if(dp[i + 1][j - 1] == true) {
                            res++;
                            dp[i][j] = true;
                        }
                    }
                }
            }
        }
        return res;
    }
};
Time Complexity: O(n^2) 
Space Complexity: O(n^2)
```
 

5. Longest Palindromic Substring
```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        int start = 0;
        int maxLen = 0;
        for(int i = n - 1; i >= 0; --i) {
            for(int j = i; j < n; ++j) {
                if(s[i] == s[j]) {
                    if(j - i <= 1 || dp[i + 1][j - 1]) {
                        dp[i][j] = true;
                        int len = j - i + 1;
                        if(len > maxLen) {
                            start = i;
                            maxLen = len;
                        }
                    }
                }
            }
        }
        return s.substr(start, maxLen);
    }
};
Time Complexity: O(n^2) 
Space Complexity: O(n^2)
```


516. Longest Palindromic Subsequence
```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for(int i = 0; i < n; ++i) dp[i][i] = 1;
        for(int i = n - 1; i >= 0; --i) {
            for(int j = i + 1; j < n; ++j) {
                if(s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1] + 2; 
                } else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][n - 1];
    }
};
Time Complexity: O(n^2) 
Space Complexity: O(n^2)
```