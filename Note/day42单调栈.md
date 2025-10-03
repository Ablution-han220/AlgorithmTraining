# Monotonic Stack
## Theory
主要特点：
单调性：:栈中的元素始终保持递增或递减的顺序。  
维护：:当有新元素入栈时，会通过出栈操作移除不符合单调性的元素，以保持栈的单调性。  
复杂度：:在解决与区间有关的问题时，单调栈可以将时间复杂度控制在O(N)，因为每个元素最多入栈一次和出栈一次。  
常见应用场景：  
解决与区间相关的问题，例如找到数组中下一个/上一个大于或小于当前元素的元素。  
在算法竞赛中常用于优化查找问题的效率。  

## Part.1
739. Daily Temperatures
```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        stack<int> st;
        vector<int> res(n, 0);
        for(int i = 0; i < n; ++i) {
            while(!st.empty() && temperatures[st.top()] < temperatures[i]) {
                res[st.top()] = i - st.top();
                st.pop();
            }
            st.push(i);
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```
 

496. Next Greater Element I
```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> map;
        for(int i = 0; i < nums1.size(); ++i) {
            map[nums1[i]] = i;
        }
        stack<int> st;
        vector<int> res(nums1.size(), -1);
        for(int i = 0; i < nums2.size(); ++i) {
            while(!st.empty() && nums2[i] > nums2[st.top()]) {
                if(map.count(nums2[st.top()]) > 0) {
                    res[map[nums2[st.top()]]] = nums2[i];
                }
                st.pop();
            }
            st.push(i);
        }
        return res;
    }
};
Time Complexity: O(n + m) 
Space Complexity: O(n)
```


503. Next Greater Element II
```c++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<int> st;
        vector<int> res(nums.size(), -1);
        for(int i = 0; i < nums.size() * 2; ++i) {
            while(!st.empty() && nums[i % nums.size()] > nums[st.top()]) {
                res[st.top()] = nums[i % nums.size()];
                st.pop();
            }
            st.push(i % nums.size());
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```