# Greedy Algorithm
## Theory
本质是选择局部最优从而达到全局最优
贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
2. 找出适合的贪心策略
3. 求解每一个子问题的最优解
4. 将局部最优解堆叠成全局最优解

## Part.3
134. Gas Station
```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int rest = 0, total = 0, start = 0;
        for(int i = 0; i < gas.size(); ++i) {
            rest += gas[i] - cost[i];
            total += gas[i] - cost[i];
            if(rest < 0) {
                rest = 0;
                start = i + 1;
            }  
        }
        if(total < 0) return -1;
        return start; 
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```
Note:  
如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站 剩油量rest[i]相加一定是大于等于零的。
每个加油站的剩余量rest[i]为gas[i] - cost[i]。

135. Candy
```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);
        // left to right, if right > left, give +1 candy
        for(int i = 1; i < ratings.size(); ++i) {
            if(ratings[i] > ratings[i - 1]) {
                candyVec[i] = candyVec[i - 1] + 1;
            }
        }
        // right to left, if left > right, ensure left has more
        for(int i = ratings.size() - 2; i >= 0; --i) {
            if(ratings[i] > ratings[i + 1]) {
                candyVec[i] = max(candyVec[i + 1] + 1, candyVec[i]);
            }
        }
        int res = 0;
        for(int c : candyVec) res += c;
        return res;
    }
};
Time Complexity: O(n) 
Space Complexity: O(n)
```
Note:  
two pass greedy

860. Lemonade Change
```c++
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0;
        for(int & bill : bills) {
            if(bill == 5) {
                five++;
            } else if (bill == 10) {
                if(five > 0) {
                    five--;
                    ten++;
                } else {
                    return false;
                }
            } else {
                if(ten > 0 && five > 0) {
                    ten--;
                    five--;
                } else if (five >= 3) {
                    five -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
Time Complexity: O(n) 
Space Complexity: O(1)
```

406. Queue Reconstruction by Height
```c++
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        if(a[0] == b[0]) {
            return a[1] < b[1];
        }
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        list<vector<int>> que;
        for(int i = 0; i < people.size(); ++i) {
            auto it = que.begin();
            int pos = people[i][1];
            while(pos--) {
                it++;
            }
            que.insert(it, people[i]);
        }
        return vector<vector<int>>(que.begin(), que.end());
    }
};
Time Complexity: O(nlogn)
Space Complexity: O(n)
```