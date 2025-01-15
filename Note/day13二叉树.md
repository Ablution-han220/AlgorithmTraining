# Binary Tree
## Part.3
110. Balanced Binary Tree
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
    int getHeight(TreeNode* node) {
        if(node == nullptr) return 0;
        int leftHeight = getHeight(node->left);
        int rightHeight = getHeight(node->right);
        if(leftHeight == -1 || rightHeight == -1) {
            return -1;
        }
        if(abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return 1 + max(leftHeight, rightHeight);
        }
    }
    bool isBalanced(TreeNode* root) {
        if(root == nullptr) return true;
        if(getHeight(root) == -1) return false;
        return true;
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) 
```
Note:
height vs depth = bottom to top vs top to bottom

257. Binary Tree Paths

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
    void backTracking(TreeNode* node, string path, vector<string>& res) {
        path += to_string(node->val);
        if(node->left == nullptr && node->right == nullptr) {
            res.emplace_back(path);
            return;
        }
        if(node->left) {
            backTracking(node->left, path + "->", res);
        }
        if(node->right) {
            backTracking(node->right, path + "->", res);
        }
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        string path;
        vector<string> res;
        if(root == nullptr) return res;
        backTracking(root, path, res);
        return res;
    }
};

Time Complexity: O(N) 
Space Complexity: O(N) in worst case
```
Note:   
backTracking for path problem.    

404. Sum of Left Leaves
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
    int sumOfLeftLeaves(TreeNode* root) {
        if(root == nullptr) return 0;
        if(root->left == nullptr && root->right == nullptr) return 0;
        int leftValue = sumOfLeftLeaves(root->left);
        if(root->left != nullptr && root->left->left == nullptr && root->left->right == nullptr) {
            leftValue = root->left->val;
        }
        int rightValue = sumOfLeftLeaves(root->right);
        int sum = leftValue + rightValue;
        return sum;
    }
};
Time Complexity: O(N) 
Space Complexity: O(N) 
```
Note:   
make sure how to determine left leaves node.

222. Count Complete Tree Nodes
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
    int countNodes(TreeNode* root) {
        if(root == nullptr) return 0;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        int leftDepth = 0;
        int rightDepth = 0;
        while(left) {
            left = left->left;
            leftDepth++;
        }
        while(right) {
            right = right->right;
            rightDepth++;
        }
        if(leftDepth == rightDepth) {
            return (2 << leftDepth) - 1;
        }
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
Time Complexity: O(logN * logN)
Space Complexity: O(N) 
```
Note:  
use complete tree feature. if the tree is a full complete binary tree, the left depth should equal to right depth.
