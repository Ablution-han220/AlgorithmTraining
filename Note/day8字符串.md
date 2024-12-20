# String
## Part.2
151. Reverse Words in a String
```c++
class Solution {
public:
    string reverseWords(string s) {
        stack<string> st;
        string res;
        string tmp;
        for(auto& ch : s) {
            if(ch != ' ') {
                tmp.push_back(ch);
                continue;
            }
            if(!tmp.empty()) {
                st.emplace(tmp);
                tmp.clear();
            }
        }
        if(!tmp.empty()) {
            st.emplace(tmp);
        }
        while(!st.empty()) {
            res += st.top();
            st.pop();
            if(!st.empty()) {
                res += " ";
            }
        }
        return res;
    }
};

Time Complexity: O(N) 
Space Complexity: O(N) stack use extra space

class Solution {
public:
    void reverse(string& s, int start, int end){ 
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {
        int slow = 0;   
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] != ' ') { 
                if (slow != 0) s[slow++] = ' '; 
                while (i < s.size() && s[i] != ' ') {
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow);
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); 
        reverse(s, 0, s.size() - 1);
        int start = 0; 
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { 
                reverse(s, start, i - 1); 
                start = i + 1; 
            }
        }
        return s;
    }
};

Time Complexity: O(N) 
Space Complexity: O(1)
```
Note:  
Step1: remove extra space in string by using two pointers method.    
Step2: reverse whole string  
Step3: reverse each words in string

