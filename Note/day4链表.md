# Linked List
## Part.2
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

160. Intersection of Two Linked Lists

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = 0;
        int lenB = 0;
        ListNode* curA = headA;
        ListNode* curB = headB;
        while(curA != nullptr) {
            curA = curA -> next;
            lenA++;
        }
        while(curB != nullptr) {
            curB = curB -> next;
            lenB++;
        }
        curA = headA;
        curB = headB;
        if(lenA < lenB) {
            swap(curA, curB);
            swap(lenA, lenB);
        }
        int diff = lenA - lenB;
        while(diff > 0) {
            curA = curA->next;
            diff--;
        }
        while(curA != nullptr) {
            if(curA == curB) {
                return curA;
            } else {
                curA = curA->next;
                curB = curB->next;
            }
        }
        return nullptr;
    }
};

Time Complexity: O(M+N) M is the length of A, N is the length of B
Space Complexity: O(1) 
```

142. Linked List Cycle II

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) {
                ListNode* ptr1 = head;
                ListNode* ptr2 = fast;
                while(ptr1 != ptr2) {
                    ptr1 = ptr1->next;
                    ptr2 = ptr2->next;
                }
                return ptr1;
            }
        }
        return nullptr;
    }
};

Time Complexity: O(N)
Space Complexity: O(1) 
```
Note:  
The most confusing part is how to find the cycle begin points.  
Need to redo later