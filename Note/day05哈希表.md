# Hash Table
## Part.1
242. Valid Anagram
```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;
        unordered_map<char, int> map;
        for(int i = 0; i < s.size(); ++i) {
             map[s[i]]++;
        }
        for(int i = 0; i < t.size(); ++i) {
            if(map.find(t[i]) == map.end() || map[t[i]] == 0) {
                return false;
            } else {
                map[t[i]]--;
            }
        }
        return true;
    }
};

Time Complexity: O(N) 
Space Complexity: O(1)
```


349. Intersection of Two Arrays

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set(nums1.begin(), nums1.end());
        unordered_set<int> resSet;
        for(int& num : nums2) {
            if(set.count(num) != 0) {
                resSet.insert(num);
            }
        }
        vector<int> res(resSet.begin(), resSet.end());
        return res;
    }
};

Time Complexity: O(M+N)
Space Complexity: O(N) 
```
Note:  
Can use array to build hase table when the data size is limited.

202. Happy Number

```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> set;
        int sum = 0;
        int flag = true;
        while(flag) {
            sum = 0;    
            while(n > 0) {
                int digit = n % 10;
                sum += digit*digit;
                n = n / 10;
            }
            n = sum;
            if(sum == 1) return true;
            if(set.count(sum) == 0) {
                set.insert(sum);
                continue;
            }
            flag = false;
        }
        return false;
    }
};

Time Complexity: O(logN)
Space Complexity: O(logN) 
```


1. Two Sum

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for(int i = 0; i < nums.size(); ++i) {
            int remain = target - nums[i];
            if(map.find(remain) == map.end()) {
                map[nums[i]] = i;
            } else {
                return {map[remain], i};
            }
        }
        return {};
    }
};

Time Complexity: O(N)
Space Complexity: O(N) 
```
