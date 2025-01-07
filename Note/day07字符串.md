# String
## Part.1
344. Reverse String
```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int left = 0;
        int right = s.size() - 1;
        while(left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }
};

Time Complexity: O(N) 
Space Complexity: O(1)
```

541. Reverse String II

```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i = 0; i < s.size(); i += 2 * k) {
            if(i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k);
            } else {
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
};

Time Complexity: O(N)
Space Complexity: O(1)
```  
