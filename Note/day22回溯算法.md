# BackTracking
## Theory
## Template
```c++
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
## Part.4
491. Non-decreasing Subsequences
```c++
class Solution {
public:
    void backTracking(vector<int>& nums, int startIndex) {
        if(path.size() > 1) res.emplace_back(path);
        unordered_set<int> uset;
        for(int i = startIndex; i < nums.size(); ++i) {
            if(uset.find(nums[i]) != uset.end() || (!path.empty() && path.back() > nums[i])) continue;
            uset.insert(nums[i]);
            path.emplace_back(nums[i]);
            backTracking(nums, i + 1);
            path.pop_back();
        }
    }

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        int startIndex = 0;
        path.clear();
        res.clear();
        backTracking(nums, startIndex);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
};
Time Complexity: O(N*2^N) 
Space Complexity: O(N)
```

46. Permutations
```c++
class Solution {
public:
    void backTracking(vector<int>& nums, int first) {
        if(first == nums.size()) {
            res.emplace_back(nums);
            return;
        }
        for(int i = first; i < nums.size(); ++i) {
            swap(nums[first], nums[i]);
            backTracking(nums, first + 1);
            swap(nums[first], nums[i]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        res.clear();
        backTracking(nums, 0);
        return res;
    }
private:
    vector<vector<int>> res;
};

Time Complexity: O(N!)
Space Complexity: O(N) 
```

47. Permutations II
```c++
class Solution {
public:
    void backTracking(vector<int>& nums, int first) {
        if(first == nums.size()) {
            res.emplace_back(nums);
            return;
        }
        vector<bool> uset(21,false);
        for(int i = first; i < nums.size(); ++i) {
            if(i > 0 && uset[nums[i] + 10] == true) continue;
            uset[nums[i] + 10] = true;
            swap(nums[first], nums[i]);
            backTracking(nums, first + 1);
            swap(nums[first], nums[i]);
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        res.clear();
        backTracking(nums, 0);
        return res;
    }
private:
    vector<vector<int>> res;
};
Time Complexity: O(n × n!)
Space Complexity: O(n x n!) 
```