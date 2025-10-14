# Monotonic Stack
## Theory
主要特点：
单调性：:栈中的元素始终保持递增或递减的顺序。  
维护：:当有新元素入栈时，会通过出栈操作移除不符合单调性的元素，以保持栈的单调性。  
复杂度：:在解决与区间有关的问题时，单调栈可以将时间复杂度控制在O(N)，因为每个元素最多入栈一次和出栈一次。  
常见应用场景：  
解决与区间相关的问题，例如找到数组中下一个/上一个大于或小于当前元素的元素。  
在算法竞赛中常用于优化查找问题的效率。  

## Part.2
42. Trapping Rain Water
```c++
class Solution {
public:
    int trap(vector<int>& height) {
        // dp O(n) O(n)
    //     int n = height.size();
    //     vector<int> leftmax(n, 0);
    //     vector<int> rightmax(n, 0);
    //     int res = 0;
    //     leftmax[0] = height[0];
    //     rightmax[n - 1] = height[n - 1];
    //     for(int i = 1; i < n; ++i) {
    //         leftmax[i] = max(leftmax[i - 1], height[i]);
    //     }
    //     for(int i = n - 2; i >= 0; --i){
    //         rightmax[i] = max(rightmax[i + 1], height[i]);
    //     }
    //     for(int i = 0; i < n; ++i) {
    //         res += min(leftmax[i], rightmax[i]) - height[i];
    //     }
    //     return res;
    // }
        // two pointers O(n) O(1)
    //     int n = height.size();
    //     int left = 0, right = n - 1;
    //     int leftmax = 0, rightmax = 0;
    //     int res = 0;
    //     while(left < right) {
    //         if(height[left] <= height[right]) {
    //             if(height[left] > leftmax) {
    //                 leftmax = height[left];
    //             } else {
    //                 res += leftmax - height[left];
    //             }
    //             left++;
    //         } else {
    //             if(height[right] > rightmax) {
    //                 rightmax = height[right];
    //             } else {
    //                 res += rightmax - height[right];
    //             }
    //             right--;
    //         }
    //     }
    // return res;
    // }
        // monotonic stack
        int n = height.size();
        stack<int> st;
        int res = 0;
        for(int i = 0; i < n; ++i) {
            while(!st.empty() && height[i] > height[st.top()]) {
                int bottom = st.top();
                st.pop();
                if(!st.empty()) {
                    int left = st.top();
                    int width = i - left - 1;
                    res += (min(height[left], height[i]) - height[bottom]) * width;
                } 
            }
            st.push(i);
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```
 

84. Largest Rectangle in Histogram
```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        stack<int> st;
        int maxArea = 0;

        for (int i = 0; i <= n; ++i) {
            int h = (i == n ? 0 : heights[i]);  // sentinel 0 height
            while (!st.empty() && h < heights[st.top()]) {
                int height = heights[st.top()];
                st.pop();
                int width = st.empty() ? i : (i - st.top() - 1);
                maxArea = max(maxArea, height * width);
            }
            st.push(i);
        }
        return maxArea;
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```
