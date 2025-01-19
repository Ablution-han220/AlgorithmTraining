# Binary Tree
## Part.4
513. Find Bottom Left Tree Value
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
    int findBottomLeftValue(TreeNode* root) {
        if(root == nullptr) return root->val;
        queue<TreeNode*> q;
        q.push(root);
        int res = 0;
        while(!q.empty()) {
            int size = q.size();
            for(int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();
                if(i == 0) res = node->val;
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
        }
        return res;
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) 
```

112. Path Sum

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
    bool backTracking(TreeNode* node, int count) {
        if(node->left == nullptr && node->right == nullptr && count == 0) return true;
        if(node->left == nullptr && node->right == nullptr) return false;
        if(node->left) {
            if(backTracking(node->left, count - node->left->val)) return true;
        }
        if(node->right) {
            if(backTracking(node->right, count - node->right->val)) return true;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return backTracking(root, targetSum - root->val);
    }
};

Time Complexity: O(N) 
Space Complexity: O(N) 
```
Note:   
When does a recursive function need to return a value? 
When is a return value not required? Here are the following three points:  
1. If you need to search the entire binary tree without dealing with recursive return values, the recursive function should not return a value.  
2. If you need to search the entire binary tree and need to handle recursive return values, the recursive function needs to return a value.  
3. If you want to search for one of the paths that meets the conditions, then the recursion must return a value, because you must return in time when you encounter a path that meets the conditions.  

113. Path Sum II
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
    void backTracking(TreeNode* node, vector<int> path, vector<vector<int>>& res, int count) {
        if(node->left == nullptr && node->right == nullptr && count == 0) {
            res.emplace_back(path);
            return;
        }
        if(node->left == nullptr && node->right == nullptr) return;
        if(node->left) {
            path.emplace_back(node->left->val);
            backTracking(node->left, path, res, count - node->left->val);
            path.pop_back();
        }
        if(node->right) {
            path.emplace_back(node->right->val);
            backTracking(node->right, path, res, count - node->right->val);
            path.pop_back();
        }
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<int> path;
        vector<vector<int>> res;
        if(root == nullptr) return res;
        path.emplace_back(root->val);
        backTracking(root, path, res, targetSum - root->val);
        return res;
    }
};
Time Complexity: O(N^2) 
Space Complexity: O(N) 
```
Note:   
make sure how to determine left leaves node.

106. Construct Binary Tree from Postorder and Inorder Traversal
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
    TreeNode* build(int left_idx, int right_idx, vector<int>& inorder, vector<int>& postorder) {
        if(left_idx > right_idx) return nullptr;
        int rootVal = postorder[post_index];
        TreeNode* root = new TreeNode(rootVal);
        int index = inorder_map[rootVal];
        post_index--;
        root->right = build(index + 1, right_idx, inorder, postorder); // build right first
        root->left = build(left_idx, index - 1, inorder, postorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.empty() || postorder.empty()) return nullptr;
        post_index = postorder.size() - 1;
        for(int i = 0; i < inorder.size(); ++i) {
            inorder_map[inorder[i]] = i;
        }
        return build(0, inorder.size() - 1, inorder, postorder);
    }
private:
    int post_index;
    unordered_map<int, int> inorder_map;
};
Time Complexity: O(N)
Space Complexity: O(N) 
```
Note:  
The right subtree should be built first, as the postorder array processes the right subtree just before the root.  

105. Construct Binary Tree from Preorder and Inorder Traversal
```C++
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
    TreeNode* build(int left_index, int right_index, vector<int>& preorder, vector<int>& inorder) {
        if(left_index > right_index) return nullptr;
        int rootVal = preorder[pre_index];
        TreeNode* root = new TreeNode(rootVal);
        int index = inorder_index[rootVal];
        pre_index++;
        root->left = build(left_index, index - 1, preorder, inorder);
        root->right = build(index + 1, right_index, preorder, inorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.empty() || inorder.empty()) return nullptr;
        pre_index = 0;
        for(int i = 0; i < inorder.size(); ++i) {
            inorder_index[inorder[i]] = i;
        }
        return build(0, inorder.size() - 1, preorder, inorder);
    }
private:
    int pre_index;
    unordered_map<int, int> inorder_index;
};
Time Complexity: O(N)
Space Complexity: O(N) 
```

