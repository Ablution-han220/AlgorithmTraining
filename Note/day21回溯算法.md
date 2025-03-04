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
## Part.3
93. Restore IP Addresses
```c++
class Solution {
public:
    void backTracking(int start, string s) {
        if(segnum == 4) {
            if(start == s.size()) {
                res.push_back(path);
            }
            // if(isValid(start, s, s.size() - 1)) {
            //     res.push_back(path);
            // }
            return;
        }
        for(int i = start; i < s.size(); ++i) {
            if(isValid(start, s, i)) {
                string tmp = s.substr(start, i - start + 1);
                int size = path.size();
                if(!path.empty()) path += '.';
                path += tmp;
                segnum++;
                backTracking(i + 1, s);
                segnum--;
                path.erase(size);
            }
        }
        return;
    }
    bool isValid(int start, string s, int end) {
        if(start > end) return false;
        if(s[start] == '0' && start != end ) return false;
        int num = 0;
        for(int i = start; i <= end; ++i) {
            if(s[i] > '9' || s[i] < '0') return false;
            num = num * 10 + (s[i] - '0');
            if(num > 255) return false;
        }
        return true;
    }
    vector<string> restoreIpAddresses(string s) {
        path.clear();
        res.clear();
        segnum = 0;
        backTracking(0, s);
        return res;
    }

private:
    string path;
    vector<string> res;
    int segnum;
};
Time Complexity: O(3^4) 
Space Complexity: O(N)
```

78. Subsets
```c++
class Solution {
public:
    void backTracking(vector<int>& nums, int startIndex) {
        res.emplace_back(path);
        if(path.size() >= nums.size()) {
            return;
        }
        for(int i = startIndex; i < nums.size(); ++i) {
            path.emplace_back(nums[i]);
            backTracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        path.clear();
        res.clear();
        backTracking(nums, 0);
        return res;
    }

private:
    vector<int> path;
    vector<vector<int>> res;

};

Time Complexity: O(N * 2^N)
Space Complexity: O(N) 
```

90. Subsets II
```c++
class Solution {
public:
    void backTracking(vector<int>& nums, int start) {
        res.emplace_back(path);
        if(path.size() == nums.size()) {
            return;
        }
        for(int i = start; i < nums.size(); ++i) {
            if(i > start && nums[i] == nums[i - 1]) continue;
            path.emplace_back(nums[i]);
            backTracking(nums, i + 1);
            path.pop_back();
        }

    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        path.clear();
        res.clear();
        sort(nums.begin(), nums.end());
        backTracking(nums, 0);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
};
Time Complexity: O(n * 2^n)
Space Complexity: O(n) 
```