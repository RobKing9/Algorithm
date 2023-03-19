# Day18 二叉树

今日内容 

● 513.找树左下角的值

 ● 112. 路径总和  113.路径总和ii

● 106.从中序与后序遍历序列构造二叉树 105.从前序与中序遍历序列构造二叉树

## [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

只需要记录最后一行第一个节点的数值就可以了。

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
    int findBottomLeftValue(TreeNode* root) {
        // 迭代，层序遍历，更新左节点的值
        // 记录层数，层数大于才更新
        if(root == nullptr) return 0;
        int res = 0;
        queue<TreeNode *> qu;
        qu.push(root);
        while(!qu.empty()) {
            int size = qu.size();
            for(int i = 0; i < size; i ++){
                auto top = qu.front();
                qu.pop();
                // 表示最左边的
                if(i == 0) res = top->val;
                if(top->left) qu.push(top->left);
                if(top->right) qu.push(top->right);
            }

        }
        return res;
    }


};
```

## [112. 路径总和](https://leetcode.cn/problems/path-sum/description/)

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。

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
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        targetSum -= root->val;
        if(root->left == nullptr && root->right == nullptr) {
            if(targetSum == 0) return true;
            else return false;
        } 
        return hasPathSum(root->left, targetSum) || hasPathSum(root->right, targetSum);
        
    }
};
```



## [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/description/)

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

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
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> res;
        vector<int> tmp;
        dfs(root, res, tmp, targetSum);
        return res;
    }
    void dfs(TreeNode* root, vector<vector<int>> &res, vector<int> &tmp, int targetSum) {   
        if(root == nullptr) return;
        tmp.push_back(root->val);
        targetSum -= root->val;
        if(root->left == nullptr && root->right == nullptr && targetSum == 0) {
            res.push_back(tmp);
        }
        dfs(root->left, res, tmp, targetSum);
        dfs(root->right, res, tmp, targetSum);
        tmp.pop_back();
        return;
    }
};
```



## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size() != postorder.size()) return nullptr;
        if(inorder.size() == 0 || postorder.size() == 0) return nullptr;

        int rootVal = postorder[postorder.size() - 1];
        int i = 0;
        for( ; i < inorder.size(); i ++ ) {
            if(inorder[i] == rootVal) break;
        }
        vector<int> inorderLeft, inorderRight, postorderLeft, postorderRight;
        for(int j = 0; j < i; j ++ ) {
            inorderLeft.push_back(inorder[j]);
            postorderLeft.push_back(postorder[j]);
        }
        for(int j = i + 1; j < inorder.size(); j ++) {
            inorderRight.push_back(inorder[j]);
            postorderRight.push_back(postorder[j - 1]);
        }
        TreeNode* root = new TreeNode(rootVal);
        root->left = buildTree(inorderLeft, postorderLeft);
        root->right = buildTree(inorderRight, postorderRight);
        return root;

    }
};
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() != inorder.size() || preorder.size() == 0 || inorder.size() == 0) return nullptr;
        int rootVal = preorder[0];
        int i = 0;
        for(; i < inorder.size(); i ++ ) {
            if(inorder[i] == rootVal) break;
        }
        cout << "find i = " << i << endl;
        vector<int> preorderLeft, preorderRight, inorderLeft, inorderRight;
        for(int j = 0; j < i; j ++ ) {
            preorderLeft.push_back(preorder[j + 1]);
            inorderLeft.push_back(inorder[j]);
        }
        cout << "left i = " << i << endl;
        for(int j = i + 1; j < inorder.size(); j ++ ) {
            preorderRight.push_back(preorder[j]);
            inorderRight.push_back(inorder[j]);
        }
        cout << "right i = " << i << endl;

        TreeNode* root = new TreeNode(rootVal);
        root->left = buildTree(preorderLeft, inorderLeft);
        root->right = buildTree(preorderRight, inorderRight);
        return root;
    }
};
```

