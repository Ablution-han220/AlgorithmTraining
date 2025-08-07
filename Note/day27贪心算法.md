# Greedy Algorithm
## Theory
本质是选择局部最优从而达到全局最优
贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
2. 找出适合的贪心策略
3. 求解每一个子问题的最优解
4. 将局部最优解堆叠成全局最优解

## Part.4
452. Minimum Number of Arrows to Burst Balloons
```c++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);
        int arrows = 1;
        int end = points[0][1];
        for(int i = 1; i < points.size(); ++i) {
            if(points[i][0] > end) {
                arrows++;
                end = points[i][1];
            }
        }
        return arrows;
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(1)
```

435. Non-overlapping Intervals
```c++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        int count = 1;
        int end = intervals[0][1];
        for(int i = 1; i < intervals.size(); ++i) {
            if(intervals[i][0] >= end) {
                count++;
                end = intervals[i][1];
            }
        }
        return intervals.size() - count;
    }
};
Time Complexity: O(nlogn) 
Space Complexity: O(1)
```
Note:  
count non-overlapping intervals, so the removal intervals = total intervals - non-overlapping intervals.

763. Partition Labels
```c++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> last(26);
        for(int i = 0; i < s.size(); ++i) {
            last[s[i] - 'a'] = i;
        }
        vector<int> res;
        int start = 0, end = 0;
        for(int i = 0; i < s.size(); ++i) {
            end = max(end, last[s[i] - 'a']);
            if(i == end) {
                res.emplace_back(end - start + 1);
                start = i + 1;
            }
        }
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```

