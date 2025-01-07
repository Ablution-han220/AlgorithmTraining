# Array
## Part.2

209. Minimum Size Subarray Sum
```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int minLength = INT_MAX;
        int left = 0;
        int sum = 0;
        for(int right = 0; right < nums.size(); ++right) {
            sum += nums[right];
            while(sum >= target) {
                minLength = min(minLength, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }
        return minLength == INT_MAX ? 0 : minLength;
    }
};

Time Complexity: O(N) in worst case the time complexity would be O(2N)
Space Complexity: O(1)
```
Note:  
The sliding window is a two-pointer method. We need think about:   
(1) How to determine the starting and ending positions?  
(2) How to expand or shrink the window? 

59. Spiral Matrix II

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // using loop invariant
        int i = 0, j = 0;
        int count = 1;
        int layer = 0;
        vector<vector<int>> res(n, vector<int>(n, 0));
        while(layer < (n + 1) / 2) {
            i = layer;
            j = layer;
            for(j; j < n - layer - 1; ++j) {
                res[i][j] = count++;
            }
            for(i; i < n - layer - 1; ++i) {
                res[i][j] = count++;
            }
            for(j; j > layer; --j) {
                res[i][j] = count++;
            }
            for(i; i > layer; --i) {
                res[i][j] = count++;
            }
            layer++;
        }
        if(n % 2) {
            res[i][j] = count;
        }
        return res;
    }
};

Time Complexity: O(N^2)
Space Complexity: O(1)
```
Note:  
Loop invariants are powerful for dealing with loops in multi-boundary problems