# Array
## Part.1
704. Binary Search
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {    
        // target in range[left, right]
        int left = 0;
        int right = nums.size() - 1;
        while(left <= right ) {
            int pivot = left + ((right - left) >> 1);
            if(nums[pivot] == target ) {
                return pivot;
            } else if(nums[pivot] > target) {
                right = pivot - 1;
            } else {
                left = pivot + 1;
            }
        }
        return -1;
    }
};

class Solution {
public:
    int search(vector<int>& nums, int target) {    
        // target in range[left, right)
        int left = 0;
        int right = nums.size();
        while(left < right ) {
            int pivot = left + ((right - left) >> 1);
            if(nums[pivot] == target ) {
                return pivot;
            } else if(nums[pivot] > target) {
                right = pivot;
            } else {
                left = pivot + 1;
            }
        }
        return -1;
    }
};

Time Complexity: O(logN)
Space Complexity: O(1)
```
Note:  
<b>For binary search, the array must be ordered.</b>  
```int middle = low + (high - low) / 2``` can prevent overflow and is functionally equivalent to ```(low + high) / 2```  
Also, use ```>>``` right shift operator ```>>1``` function is equal to ```/2```.But pay attention to the priority order, shift operator priority is lower than ```+ - x / ```etc.  

27. Remove Element

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int fast = 0;
        int slow = 0;
        for( fast = 0; fast < nums.size(); ++fast ) {
            if( nums[fast] != val ) {
                nums[slow] = nums[fast];
                ++slow;
            }
        }
        return slow;
    }
};

Time Complexity: O(N)
Space Complexity: O(1)
```

977. Squares of a Sorted Array

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> res(nums.size(), 0);
        int left = 0;
        int right = nums.size() - 1;
        int resRight = right;
        while (left <= right) {
            int squareLeft = nums[left] * nums[left];
            int squareRight = nums[right] * nums[right];
            if(squareLeft <= squareRight ) {
                res[resRight] = squareRight;
                right--;
            } else {
                res[resRight] = squareLeft;
                left++;
            }
            resRight--;
        }
        return res;
    }
};

Time Complexity: O(N)
Space Complexity: O(N)
```
Note:  
For two pointers, the key is defining each pointer's meaning.