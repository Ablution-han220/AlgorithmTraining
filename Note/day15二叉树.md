# Binary Tree
## Part.5
654. Maximum Binary Tree
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
    TreeNode* build(int left, int right, vector<int>& nums) {
        if(left > right) return nullptr;
        int maxIndex = left;
        for(int i = left; i <= right; ++i) {
            if(nums[i] > nums[maxIndex]) maxIndex = i;
        }
        TreeNode* root = new TreeNode(nums[maxIndex]);
        root->left = build(left, maxIndex - 1, nums);
        root->right = build(maxIndex + 1, right, nums);
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return build(0, nums.size() - 1, nums);
    }
};
Time Complexity: O(N^2) 
Space Complexity: O(N) 

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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        stack<TreeNode*> st;
        for(int& num : nums) {
            TreeNode* node = new TreeNode(num);
            while(!st.empty() && st.top()->val < node->val) {
                node->left = st.top();
                st.pop();
            }
            if(!st.empty()) {
                st.top()->right = node;
            }
            st.push(node);
        }
        while(st.size() > 1) {
            st.pop();
        }
        return st.top();
    }
};
Time Complexity: O(N) using a monotonic stack
```

617. Merge Two Binary Trees

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
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == nullptr) return root2;
        if(root2 == nullptr) return root1;
        TreeNode* root = new TreeNode();
        root->val = root1->val + root2->val;
        root->left = mergeTrees(root1->left, root2->left);
        root->right = mergeTrees(root1->right, root2->right);
        return root;
    }
};

Time Complexity: O(M) A total of m nodes need to be traversed. Here, m represents the minimum number of nodes from the two given trees.
Space Complexity: O(N) 
```

700. Search in a Binary Search Tree
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
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root) {
            if(root->val == val) return root;
            if(root->val > val) {
                root = root->left;
            } else {
               root = root->right; 
            }
        }
        return nullptr;
    }
};
Time Complexity: O(H) H is the height of the tree.
Space Complexity: O(H) 
```
Note:   
for Binary Search Tree(BST):   
A valid BST is defined as follows:
1.The left subtree of a node contains only nodes with keys less than the node's key.   
2.The right subtree of a node contains only nodes with keys greater than the node's key.   
3.Both the left and right subtrees must also be binary search trees.   

98. Validate Binary Search Tree
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
    TreeNode* prev = nullptr;
    bool isValidBST(TreeNode* root) {
        if(root == nullptr) return true;
        bool left = isValidBST(root->left);
        if(prev != nullptr && prev->val >= root->val) {
            return false;
        }
        prev = root;
        bool right = isValidBST(root->right);
        return left && right;
    }
};
Time Complexity: O(N)
Space Complexity: O(N) 
```
Note:  
trap: All nodes in the left subtree are smaller than the middle node, and all nodes in the right subtree are larger than the middle node. Cannot simply compare left value < middle value < right value.  
