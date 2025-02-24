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
## Part.2
39. Combination Sum
```c++
class Solution {
public:
    void backTracking(int start, vector<int>& candidates, int target) {
        if(sum > target) return;
        if(sum == target) {
            res.emplace_back(path);
            return;
        }
        for(int i = start; i < candidates.size(); ++i) {
            path.emplace_back(candidates[i]);
            sum += candidates[i];
            backTracking(i, candidates, target);
            sum -= candidates[i];
            path.pop_back();
        }
        return;
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        path.clear();
        res.clear();
        sum = 0;
        backTracking(0, candidates, target);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
    int sum;
};
Time Complexity: O(n * 2^n) 
Space Complexity: O(target)
```

40. Combination Sum II
```c++
class Solution {
public:
    void backTracking(int start, vector<int>& candidates, int target) {
        if(sum == target) {
            res.emplace_back(path);
            return;
        }
        for(int i = start; i < candidates.size() && sum + candidates[i] <= target; ++i) {
            if(i > start && candidates[i] == candidates[i - 1]) continue;
            sum += candidates[i];
            path.emplace_back(candidates[i]);
            backTracking(i + 1, candidates, target);
            sum -= candidates[i];
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        path.clear();
        res.clear();
        sum = 0;
        sort(candidates.begin(), candidates.end());
        backTracking(0, candidates, target);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
    int sum;
};
Time Complexity: O(N * 2^N)
Space Complexity: O(N) 
```


131. Palindrome Partitioning
```c++
class Solution {
public:
    void backTracking(int start, string s) {
        if(start >= s.size()) {
            res.emplace_back(path);
            return;
        }
        for(int i = start; i < s.size(); ++i) {
            if(isPalindrome[start][i]) {
                string str = s.substr(start, i - start + 1);
                path.push_back(str);
            } else {
                continue;
            }
            backTracking(i + 1, s);
            path.pop_back();
        }
        return;
    }
    void computePalindrome(string s) {
        isPalindrome.resize(s.size(), vector<bool>(s.size(),false));
        for(int i = s.size() - 1; i >= 0; --i) {
            for(int j = i; j < s.size(); ++j) {
                if(j == i) {
                    isPalindrome[i][j] = true;
                } else if(j - i == 1) {
                    isPalindrome[i][j] = (s[i] == s[j]);
                } else {
                    isPalindrome[i][j] = (s[i] == s[j] && isPalindrome[i+1][j-1]);
                } 
            }
        }
    }
    vector<vector<string>> partition(string s) {
        path.clear();
        res.clear();
        computePalindrome(s);
        backTracking(0, s);
        return res;
    }
private:
    vector<string> path;
    vector<vector<string>> res;
    vector<vector<bool>> isPalindrome;
};
Time Complexity: O(n * 2^n)
Space Complexity: O(n^2) 
```