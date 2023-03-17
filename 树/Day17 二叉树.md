# Day17 二叉树

今日内容： 

● 110.平衡二叉树 

● 257. 二叉树的所有路径 

● 404.左叶子之和 

## [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/description/)

```cpp
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
    vector<string> binaryTreePaths(TreeNode* root) {
        if(root == nullptr) return {};
        vector<string> res;
        vector<int> path;
        getPath(root, path, res);
        return res;
    }
    void getPath(TreeNode* root, vector<int> &path, vector<string> &res) {
        if(root == nullptr) return;
        // 结束条件
        if(root->left == nullptr && root->right == nullptr) {
            string s;
            for(int i = 0; i < path.size(); i ++ ) {
                s += to_string(path[i]);
                s += "->";
            }
            // 加入最后一个节点
            s += to_string(root->val);
            res.push_back(s);
            return;
        }
        // 写在这里是为了避免 加入最后一个节点
        path.push_back(root->val);
        // 递归 处理左右节点
        getPath(root->left, path, res);
        getPath(root->right, path, res);
        // 回溯
        path.pop_back();
        return;
    }
};
```

非递归，用两个栈。另一个栈记录到当前的路径。

```cpp
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
    vector<string> binaryTreePaths(TreeNode* root) {
        if(root == nullptr) return {};
        vector<string> res;
        // 存放到当前的路径
        stack<string> path;
        stack<TreeNode *> st;
        st.push(root);
        path.push(to_string(root->val));

        while(!st.empty()) {
            auto top = st.top();
            st.pop();
            auto topPath = path.top();
            path.pop();

            if(top->left == nullptr && top->right == nullptr) res.push_back(topPath);

            if(top->left) {
                st.push(top->left);
                path.push(topPath + "->" + to_string(top->left->val));
            }
            if(top->right) {
                st.push(top->right);
                path.push(topPath + "->" + to_string(top->right->val));
            }
        }
        return res;

    }
};
```



## [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

```cpp
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
    bool isBalanced(TreeNode* root) {
        // 求出根节点的左右子树最大高度，满足绝对值不超过1，然后递归左右子树
        // 这样的方式下去 要么最大高度相差太大，要么在左右子树中
        if(root == nullptr) return true;
        return abs(maxDepth(root->left) - maxDepth(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right);
    }
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }

};
```



## [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/description/)

给定二叉树的根节点 `root` ，返回所有左叶子之和。

```cpp
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
        int res = 0;
        getLeftSum(root, res);
        return res;
    }
    void getLeftSum(TreeNode* root, int &res) {
        if(root == nullptr) return;
        // 注意结束 只能试叶子节点
        if(root->left && root->left->left == nullptr && root->left->right == nullptr)
             res += root->left->val;
        getLeftSum(root->left, res);
        getLeftSum(root->right, res);
        return;
    }
};
```

