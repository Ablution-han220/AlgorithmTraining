# Linked List
## Part.1
203. Remove Linked List Elements
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummy = new ListNode(-1, head);
        ListNode* cur = dummy;
        while(cur->next != nullptr) {
            if(cur->next->val == val) {
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            } else {
                cur = cur->next;
            }
        }
        head = dummy->next;
        delete dummy;
        return head;
    }
};

Time Complexity: O(N) 
Space Complexity: O(1)
```
Note:  
Using a dummy node is a smart way to deal with the head node.  
Need to pay attention to manual release of memory on C++.

707. Design Linked List

```c++
class MyLinkedList {
public:
    MyLinkedList() {
        dummy = new ListNode(-1);
        size = 0;    
    }
    
    int get(int index) {
        if(index > (size - 1) || index < 0) {
            return -1;
        }
        ListNode* cur = dummy->next;
        while(index > 0) {
            cur = cur->next;
            index--;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        ListNode* add = new ListNode(val);
        add->next = dummy->next;
        dummy->next = add;
        size++;
    }
    
    void addAtTail(int val) {
        ListNode* add = new ListNode(val);
        ListNode* cur = dummy;
        while(cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = add;
        size++;
    }
    
    void addAtIndex(int index, int val) {
        if(index > size) return;
        if(index < 0) index = 0;
        ListNode* cur = dummy;
        while(index > 0) {
            cur = cur->next;
            index--;
        }
        ListNode* add = new ListNode(val);
        add->next = cur->next;
        cur->next = add;
        size++;
    }
    
    void deleteAtIndex(int index) {
        if(index >= size || index < 0) {
            return;
        }
        ListNode* cur = dummy;
        while(index > 0) {
            cur = cur->next;
            index--;
        }
        ListNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        tmp = nullptr;
        size--;
    }
private:
    struct ListNode {
        int val;
        ListNode* next;
        ListNode(int val): val(val), next(nullptr) {}
    };
    ListNode* dummy;
    int size;
};

Time Complexity: O(index)
Space Complexity: O(N)
```
Note:  
Notice the different positions of each different operation.  
Delete pointer in C++ only releases memory, not value. So, to prevent problems, we need to set the pointer value to null.

206. Reverse Linked List

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // iterative
        ListNode* tmp = nullptr;
        ListNode* cur = head;
        ListNode* pre = nullptr;
        while(cur != nullptr) {
            tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
};

Time Complexity: O(N)
Space Complexity: O(1)

class Solution {
public:
    ListNode* reverse(ListNode* cur, ListNode* pre) {
        if(cur == nullptr) return pre;
        ListNode* tmp = cur->next;
        cur->next = pre;
        return reverse(tmp, cur);
    }

    ListNode* reverseList(ListNode* head) {
        //recursive
        ListNode* cur = head;
        ListNode* pre = nullptr;
        return reverse(cur, pre);
    }
};

Time Complexity: O(N)
Space Complexity: O(N) Call n-level stack space
```