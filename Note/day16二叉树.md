# Binary Tree
## Part.6
530. Minimum Absolute Difference in BST
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
    void traversal(TreeNode* cur) {
        if(cur == nullptr) return;
        traversal(cur->left);
        if(pre != nullptr) {
            result = min(result, cur->val - pre->val);
        }
        pre = cur;
        traversal(cur->right);
    }
    int getMinimumDifference(TreeNode* root) {
        traversal(root);
        return result;
    }
private:
    int result = INT_MAX;
    TreeNode* pre = nullptr;
};
Time Complexity: O(N) 
Space Complexity: O(N) 
```

501. Find Mode in Binary Search Tree

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
    void traversal(TreeNode* cur) {
        if(cur == nullptr) return;
        traversal(cur->left);
        if(pre != nullptr) {
            if(pre->val == cur->val) {
                count++;
            } else {
                count = 1;
            }
        }
        pre = cur;
        if(count == maxCount) {
            res.emplace_back(cur->val);
        }
        if(count > maxCount) {
            maxCount = count;
            res.clear();
            res.emplace_back(cur->val);
        }
        traversal(cur->right);
    }
    vector<int> findMode(TreeNode* root) {
        res.clear();
        traversal(root);
        return res;
    }
private:
    TreeNode* pre = nullptr;
    int maxCount = 0;
    int count = 1;
    vector<int> res;
};

Time Complexity: O(N) 
Space Complexity: O(N) 
```

236. Lowest Common Ancestor of a Binary Tree
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return nullptr;
        if(root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(left != nullptr && right != nullptr) return root;
        if(left == nullptr && right != nullptr) return right;
        if(right == nullptr && left != nullptr) return left;
        return nullptr;
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) 
```