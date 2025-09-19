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

## Part.10
300. Longest Increasing Subsequence
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        // dp time:O(n^2)
        // vector<int> dp(nums.size(), 1);
        // int res = 1;
        // for(int i = 1; i < nums.size(); ++i) {
        //     for(int j = 0; j < i; j++) {
        //         if(nums[i] > nums[j]) {
        //             dp[i] = max(dp[i], dp[j] + 1);
        //         }
        //     }
        //     res = max(res, dp[i]);
        // }
        // return res;

        // greedy + binary search
        vector<int> sub;
        for(int num : nums) {
            int left = 0, right = sub.size() - 1;
            int pos = sub.size();
            while(left <= right) {
                int mid = left + (right - left) / 2;
                if(sub[mid] >= num) {
                    pos = mid;
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            if(pos == sub.size()) {
                sub.emplace_back(num);
            } else {
                sub[pos] = num;
            }
        }
        return sub.size();
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(n)
```
 

674. Longest Continuous Increasing Subsequence
```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        // greedy time: O(n) space:O(1)
        int res = 1;
        int len = 1;
        for(int i = 1; i < nums.size(); ++i) {
            if(nums[i] > nums[i - 1]) {
                len++;
            } else {
                len = 1;
            }
            res = max(res, len);
        }
        return res;
    }
        // dp time: O(n) space:O(n)
    //     vector<int> dp(nums.size(), 1);
    //     int res = 1;
    //     for(int i = 1; i < nums.size(); ++i) {
    //         if(nums[i] > nums[i - 1]) {
    //             dp[i] = dp[i - 1] + 1;
    //         }
    //         res = max(dp[i], res);
    //     }
    //     return res;
    // }
};
Time Complexity: O(n)
Space Complexity: O(1)
```


718. Maximum Length of Repeated Subarray
```c++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        // 2d dp time:O(n*m) space:O(n*m)
    //     int n = nums1.size(), m = nums2.size();
    //     vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    //     int res = 0;
    //     for(int i = 1; i <= n; ++i) {
    //         for(int j = 1; j <= m; ++j) {
    //             if(nums1[i - 1] == nums2[j - 1]) {
    //                 dp[i][j] = dp[i - 1][j - 1] + 1;
    //                 res = max(res, dp[i][j]);
    //             }
    //         }
    //     }
    //     return res;
    // }
        // 1d dp
        int n = nums1.size(), m = nums2.size();
        vector<int> dp(n + 1, 0);
        int res = 0;
        for(int i = 1; i <= n; ++i) {
            for(int j = m; j >= 1; --j) {
                if(nums1[i - 1] == nums2[j - 1]) {
                    dp[j] = dp[j - 1] + 1;
                    res = max(res, dp[j]);
                } else {
                    dp[j] = 0;
                }
            }
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1) 
```