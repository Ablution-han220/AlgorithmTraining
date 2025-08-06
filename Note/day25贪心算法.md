# Greedy Algorithm
## Theory
本质是选择局部最优从而达到全局最优
贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
2. 找出适合的贪心策略
3. 求解每一个子问题的最优解
4. 将局部最优解堆叠成全局最优解

## Part.2 
121. Best Time to Buy and Sell Stock
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy_price = prices[0];
        int maxProfit = 0;
        for(int i = 1; i < prices.size(); ++i) {
            buy_price = min(buy_price, prices[i]);
            maxProfit = max(maxProfit, prices[i] - buy_price);
        }
        return maxProfit;
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
        int profit = 0;
        for(int i = 1; i < prices.size(); ++i) {
            if(prices[i] - prices[i - 1] > 0) {
                profit += prices[i] - prices[i - 1];
            }
        }
        return profit;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```
Note:  
Profit split is the key point


55. Jump Game
```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int furthest = 0;
        for(int i = 0; i < nums.size(); ++i) {
            if(i > furthest) return false;
            furthest = max(furthest, nums[i] + i);
            if(furthest >= nums.size() - 1) return true;
        }
        return true;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```
If you ever reach an index i > furthest, you’re stuck.

45. Jump Game II
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int jump = 0, end = 0, farthest = 0;
        for(int i = 0; i < nums.size() - 1; ++i) {
            farthest = max(farthest, nums[i] + i);
            if(i == end) {
                ++jump;
                end = farthest;
            }
        }
        return jump;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```
Note:  
We stop at nums.size() - 2 because:  
❗️ We never need to jump from the last index.

1005. Maximize Sum Of Array After K Negations
```c++
class Solution {
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp);
        for(int i = 0; i < nums.size(); ++i) {
            if(nums[i] < 0 && k > 0) {
                nums[i] *= -1;
                --k; 
            }
        }
        if(k % 2 == 1) nums[nums.size() - 1] *= -1;
        int res = 0;
        for(int& n : nums) res += n;
        return res;
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(1)
```
Note:  
那么本题的解题步骤为：

第一步：将数组按照绝对值大小从大到小排序，注意要按照绝对值的大小
第二步：从前向后遍历，遇到负数将其变为正数，同时K--
第三步：如果K还大于0，那么反复转变数值最小的元素，将K用完
第四步：求和