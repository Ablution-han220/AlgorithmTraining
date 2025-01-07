# Stack and Queue
## Part.2
150. Evaluate Reverse Polish Notation
```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long long> num;
        for(string &s : tokens) {
            if(s == "+" || s == "-" || s == "*" || s == "/") {
                long long num2 = num.top();
                num.pop();
                long long num1 = num.top();
                num.pop();
                if(s == "+") num.push(num1 + num2);
                else if(s == "-") num.push(num1 - num2);
                else if(s == "*") num.push(num1 * num2);
                else num.push(num1 / num2);
            } else {
                num.push(stoll(s));
            }
        }
        return num.top();
    }
};

Time Complexity: O(N)
Space Complexity: O(N)
```
Note:  
note out-of-bounds problem for 32-bit integer.

239. Sliding Window Maximum
```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> maxQ;
        for(int i = 0; i < nums.size(); ++i) {
            int num = nums[i];
            // Maintain Deque in Descending Order
            while(!maxQ.empty() && maxQ.back() < num) {
                maxQ.pop_back();
            }
            maxQ.push_back(num);
            // Remove an Element That Is Out of the Current Window
            if(i >= k && nums[i - k] == maxQ.front()) {
                maxQ.pop_front();
            }
            // Append Maximum of Current Window to Result
            if(i >= k - 1) {
                res.emplace_back(maxQ.front());
            }
        }
        return res;
    }
};

Time Complexity: O(N) 
Space Complexity: O(k)
```
Note:   
step1: Maintain Deque in Descending Order   
step2: Remove an Element That Is Out of the Current Window   
step3: Append Maximum of Current Window to Result   

347. Top K Frequent Elements
```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> map;
        for(int &num : nums) {
            map[num]++;
        }
        // min heap
        auto cmp = [] (pair<int,int>& a, pair<int,int>& b) {
            return a.second > b.second;
        };
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> min_heap;
        for(auto& it : map) {
            min_heap.push(it);
            if(min_heap.size() > k) {
                min_heap.pop();
            }
        }
        vector<int>res;
        for(int i = k - 1; i >= 0; --i) {
            res.emplace_back(min_heap.top().first);
            min_heap.pop();
        }
        return res;
    }
};

Time Complexity: O(NlogK)  insert operate in min_heap cost logK
Space Complexity: O(N)
```

Note:   
priority_queue(heap) in C++ is an ideal data structure to solve the top k frequency problem.