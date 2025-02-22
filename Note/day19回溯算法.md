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
## Part.1
77. Combinations
```c++
class Solution {
public:
    void backTracking(int start, int n, int k) {
        if(path.size() == k) {
            res.emplace_back(path);
            return;
        }
        for(int i = start; i <= n - (k - path.size()) + 1; ++i) {
            path.emplace_back(i);
            backTracking(i + 1, n, k);
            path.pop_back();
        }
        return;
    }

    vector<vector<int>> combine(int n, int k) {
        path.clear();
        res.clear();
        backTracking(1, n, k);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
};
Time Complexity: O(n * k) //n is the number of elements and k is the size of the subset. The backtrack function is called n times, because there are n possible starting points for the subset. For each starting point, the backtrack function iterates through all k elements. This is because the comb list must contain all k elements in order for it to be a valid subset.
 Space Complexity: O(k) //The comb list stores at most k elements. This is because the backtrack function only adds elements to the comb list when the subset is not yet complete.
```

216. Combination Sum III

```c++
class Solution {
public:
    void backTracking(int start, int k, int n) {
        if(sum > n) return;
        if(path.size() == k) {
            if(sum == n) res.emplace_back(path);
            return;
        }
        for(int i = start; i <= 9 - (k - path.size()) + 1; ++i) {
            sum += i;
            path.emplace_back(i);
            backTracking(i + 1, k, n);
            sum -= i;
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        path.clear();
        res.clear();
        sum = 0;
        backTracking(1, k, n);
        return res;
    }
private:
    vector<int> path;
    vector<vector<int>> res;
    int sum;
};
Time Complexity: O(k⋅C(9,k))
Space Complexity: O(k) 
```
There is a general formula for analyzing the time complexity of backtracking problems: path length × number of leaves in the search tree

538. Convert BST to Greater Tree
```c++
class Solution {
public:
    void backTracking(int start, string digits) {
        if(path.size() == digits.size()) {
            res.emplace_back(path);
            return;
        }
        char digit = digits[start];
        string letter = map[digit];
        for(int i = 0; i < letter.size(); ++i) {
            path += letter[i];
            backTracking(start + 1, digits);
            path.pop_back();
        }
        return;
    }
    vector<string> letterCombinations(string digits) {
        path.clear();
        res.clear();
        if(digits.empty()) return res;
        backTracking(0, digits);
        return res;
    }
private:
    unordered_map<char, string> map = 
    {
        {'2', "abc"},
        {'3', "def"},
        {'4', "ghi"},
        {'5', "jkl"},
        {'6', "mno"},
        {'7', "pqrs"},
        {'8', "tuv"},
        {'9', "wxyz"}
    };
    string path;
    vector<string> res;
};
Time Complexity: O(3^m * 4^n)
Space Complexity: O(m + n) 
```