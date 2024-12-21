# Stack and Queue
## Part.1
232. Implement Queue using Stacks
```c++
class MyQueue {
    std::stack<int>a,b;
public:
    MyQueue() {
        
    }
    
    void push(int x) {
        while(!b.empty()){
            a.push(b.top());
            b.pop();
        }
        a.push(x);
        while(!a.empty()){
            b.push(a.top());
            a.pop();
        }
    }
    
    int pop() {
        int temp = b.top();
        b.pop();
        return temp;
    }
    
    int peek() {
        return b.top();
    }
    
    bool empty() {
        return b.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */

Time Complexity: O(1) for push() and empty() operation, O(N) for pop() and peak() operation
Space Complexity: O(N)
```

225. Implement Stack using Queues
```c++
class MyStack {
public:
    MyStack() {
        
    }
    
    void push(int x) {
        q.emplace(x);
    }
    
    int pop() {
        int n = q.size();
        for(int i = 0; i < n - 1; ++i) {
            int top = q.front();
            q.emplace(top);
            q.pop();
        }
        int res = q.front();
        q.pop();
        return res;

    }
    
    int top() {
        int res = pop();
        q.emplace(res);
        return res;
    }
    
    bool empty() {
        if(q.empty()) {
            return true;
        }
        return false;
    }
private:
    queue<int> q;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */

Time Complexity: O(1) for push() and empty() operation, O(N) for pop() and top() operation
Space Complexity: O(N)
```

20. Valid Parentheses