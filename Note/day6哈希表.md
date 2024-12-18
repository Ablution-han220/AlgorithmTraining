# Hash Table
## Part.2
454. 4Sum II
```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> mapA;
        for(int& a : nums1) {
            for(int& b : nums2) {
                mapA[a + b]++;
            }
        }
        int count = 0;
        for(int& c : nums3) {
            for(int& d : nums4) {
                if(mapA.find(0 - (c + d)) != mapA.end()) {
                    count = count + mapA[0 - (c + d)];
                }
            }
        }
        return count;
    }
};

Time Complexity: O(N^2) 
Space Complexity: O(N^2)
```


383. Ransom Note

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> map;
        for(char& c : magazine) {
            map[c]++;
        }
        for(char& c : ransomNote) {
            if(map.find(c) == map.end() || map[c] == 0) {
                return false;
            }
            map[c]--;
        }
        return true;
    }
};

Time Complexity: O(N)
Space Complexity: O(1), O(N) n the lowercase of English letter.
```

15. 3Sum

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size(); ++i) {
            if(nums[i] > 0) return res;
            if(i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while(left < right) {
                if(nums[i] + nums[left] + nums[right] > 0) right--;
                else if(nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    vector<int> sub = {nums[i], nums[left], nums[right]};
                    res.emplace_back(sub);
                    while(left < right && nums[left] == nums[left + 1]) left++;
                    while(left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
};

Time Complexity: O(N^2)
Space Complexity: O(1) 
```
Note:  
How to use two pointers metheds?  
step1: sort the array  
step2: define left and right pointer  
step3: move left or right pointer. (make sure to remove duplicate elements)
step4: push valid result into array.

18. 4Sum

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for(int k = 0; k < nums.size(); ++k) {
            if(nums[k] > target && nums[k] >= 0) break;
            if(k > 0 && nums[k] == nums[k - 1]) continue;
            for(int i = k + 1; i < nums.size(); ++i) {
                if(nums[i] + nums[k] > target && nums[i] + nums[k] >= 0) break;
                if(i > k + 1 && nums[i] == nums[i - 1]) continue;
                int left = i + 1;
                int right = nums.size() - 1;
                while(left < right) {
                    if((long)nums[k] + nums[i] + nums[left] + nums[right] > target) right--;
                    else if((long)nums[k] + nums[i] + nums[left] + nums[right] < target) left++;
                    else {
                        vector<int> sub = {nums[k],nums[i], nums[left], nums[right]};
                        res.emplace_back(sub);
                        while(left < right && nums[left] == nums[left + 1]) left++;
                        while(left < right && nums[right] == nums[right - 1]) right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return res;
    }
};
Time Complexity: O(N^3)
Space Complexity: O(1) 
```
Note:  
The extension version of 3sum question, pay attenton to how to remove duplicate elements.  
Pruning is also very important in order to reduce time complexity.
