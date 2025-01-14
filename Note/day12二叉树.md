# Binary Tree
## Part.2
226. Invert Binary Tree
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
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return nullptr;
        TreeNode* left = invertTree(root->left);
        TreeNode* right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) in worst case

```

101. Symmetric Tree

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
    bool isMirror(TreeNode* left, TreeNode* right) {
        if(left == nullptr && right == nullptr) {
            return true;
        }
        if(left == nullptr || right == nullptr) {
            return false;
        }
        return left->val == right->val && isMirror(left->left, right->right) && isMirror(left->right, right->left);
    }
    bool isSymmetric(TreeNode* root) {
        return isMirror(root->left, root->right);
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) in worst case
```
Note:   
key idea: use post-order traversal to compare left and right side at the same time.   

111. Minimum Depth of Binary Tree
```c++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int depth = 1;
        while(!q.empty()) {
            int size = q.size();
            for(int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();
                if(node->left == nullptr && node->right == nullptr) {
                    return depth;
                }
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            depth++;
        }
        return depth;
    }
};

Time Complexity: O(N) 
Space Complexity: O(N) 
```

104. Maximum Depth of Binary Tree
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int depth = 0;
        while(!q.empty()) {
            int size = q.size();
            for(int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            ++depth;
        }
        return depth;
    }
};
Time Complexity: O(N)
Space Complexity: O(N) 
```

