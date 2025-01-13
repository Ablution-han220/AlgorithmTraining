# Binary Tree
## Part.1
### Recursion

```c++
class Solution {
public:
    // prorder
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // mid
        traversal(cur->left, vec);  // left
        traversal(cur->right, vec); // right
    }

    // inorder
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        traversal(cur->left, vec);  // left
        vec.push_back(cur->val);    // mid
        traversal(cur->right, vec); // right
    }

    //postorder
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        traversal(cur->left, vec);  // left
        traversal(cur->right, vec); // right
        vec.push_back(cur->val);    // mid
    }

    vector<int> Traversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};

```
Note:  
144. Binary Tree Preorder Traversal
94. Binary Tree Inorder Traversal
145. Binary Tree Postorder Traversal

follow up: Morris traversal uses no extra space to traverse the tree.

### Iterative
```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); 
                if (node->right) st.push(node->right);  
                if (node->left) st.push(node->left); 
                st.push(node);                          
                st.push(NULL);   
            } else { 
                st.pop();           
                node = st.top();    
                st.pop();
                result.push_back(node->val); 
            }
        }
        return result;
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
                if (node->right) st.push(node->right);  // 添加右节点（空节点不入栈）

                st.push(node);                          // 添加中节点
                st.push(NULL); // 中节点访问过，但是还没有处理，加入空节点做为标记。

                if (node->left) st.push(node->left);    // 添加左节点（空节点不入栈）
            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.top();    // 重新取出栈中元素
                st.pop();
                result.push_back(node->val); // 加入到结果集
            }
        }
        return result;
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); 
                st.push(node);                          
                st.push(NULL);
                if (node->right) st.push(node->right);  
                if (node->left) st.push(node->left);    
            } else { 
                st.pop();           
                node = st.top();    
                st.pop();
                result.push_back(node->val); 
            }
        }
        return result;
    }
};
```
Note:   
The order in the iteration stack is reversed from the original order.

### Level order traversal
102. Binary Tree Level Order Traversal
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // iterative
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> q;
        if (root != NULL) q.push(root);
        vector<vector<int>> result;
        while (!q.empty()) {
            int size = q.size();
            vector<int> vec;
            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();
                vec.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
    // recursion
    void order(TreeNode* cur, vector<vector<int>>& result, int depth)
    {
        if (cur == nullptr) return;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int depth = 0;
        order(root, result, depth);
        return result;
    }
};
Time Complexity: O(N)  Every node is visited exactly once.
Space Complexity: O(W) The maximum width of the tree (number of nodes in the largest level) determines the maximum size of the queue.
```

Note:   
107. Binary Tree Level Order Traversal II
199. Binary Tree Right Side View
637. Average of Levels in Binary Tree
429. N-ary Tree Level Order Traversal
515. Find Largest Value in Each Tree Row
116. Populating Next Right Pointers in Each Node
```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if(root == nullptr) return root;
        Node* leftmost = root;
        while(leftmost->left) {
            Node* head = leftmost;
            while(head) {
                head->left->next = head->right;
                if(head->next) {
                    head->right->next = head->next->left;
                }
                head = head->next; 
            }
            leftmost = leftmost->left;
        }
        return root;
    }
};

Time Complexity: O(N)
Space Complexity: O(1) 
```
117. Populating Next Right Pointers in Each Node II
104. Maximum Depth of Binary Tree
111. Minimum Depth of Binary Tree

