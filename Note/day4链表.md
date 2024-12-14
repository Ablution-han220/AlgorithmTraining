Linked List

24. Swap Nodes in Pairs
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
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(-1, head);
        ListNode* cur = dummy;
        while(cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* tmp1 = cur->next;
            ListNode* tmp2 = cur->next->next;
            tmp1->next = tmp2->next;
            tmp2->next = tmp1;
            cur->next = tmp2;
            cur = cur->next->next;
        }
        return dummy->next;
    }
};

Time Complexity: O(N) 
Space Complexity: O(1)
```
Note:  

19. Remove Nth Node From End of List

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(-1,head);
        ListNode* cur = dummy;
        ListNode* fast = dummy;
        while(n > 0) {
            fast = fast->next;
            n--;
        }
        while(fast->next != nullptr) {
            cur = cur->next;
            fast = fast->next;
        }
        cur->next = cur->next->next;
        return dummy->next;
    }
};

Time Complexity: O(N)
Space Complexity: O(1)
```
Note:  
Easy to implement two pointer method

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