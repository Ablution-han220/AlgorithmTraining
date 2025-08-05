# Greedy Algorithm
## Theory
本质是选择局部最优从而达到全局最优
贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
2. 找出适合的贪心策略
3. 求解每一个子问题的最优解
4. 将局部最优解堆叠成全局最优解

## Part.1 
455. Assign Cookies
```c++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = 0;
        for(int i = 0; i < s.size(); ++i) {
            if(index < g.size() && s[i] >= g[index]) {
                index++;
            }
        }
        return index;
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(1)
```

376. Wiggle Subsequence
```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size() < 2) return nums.size();
        int up = 1, down = 1;
        for(int i = 1; i < nums.size(); ++i) {
            if(nums[i] > nums[i - 1]) up = down + 1;
            if(nums[i] < nums[i - 1]) down = up + 1;
        }
        return max(up, down);
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```
Note:  
track the peak and valley


53. Maximum Subarray
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int current_sum = nums[0];
        int global_sum = nums[0];
        for(int i = 1; i < nums.size(); ++i) {
            current_sum = max(nums[i], current_sum + nums[i]);
            global_sum = max(current_sum, global_sum);
        }
        return global_sum;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```