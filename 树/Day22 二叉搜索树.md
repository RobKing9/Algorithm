# Day22 二叉搜索树

今日内容： 

 ● 235. 二叉搜索树的最近公共祖先 

● 701.二叉搜索树中的插入操作 

 ● 450.删除二叉搜索树中的节点 

## [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

```cpp
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
        if(root == nullptr || p == nullptr || q == nullptr) return nullptr;
        return helper(root, p, q);
    }
    TreeNode* helper(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return nullptr;
        // 如果都大于 遍历右边
        if(q->val > root->val && p->val > root->val) return helper(root->right, p, q);
        // 如果都小于，遍历左边
        else if(q->val < root->val && p->val < root->val) return helper(root->left, p, q);
		// 否则返回根节点
        return root;
    }
};
```

## [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)

给定二叉搜索树（BST）的根节点 `root` 和要插入树中的值 `value` ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 **保证** ，新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 **任意有效的结果** 。

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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        // 如果为空 创建一个新节点返回
        if(root == nullptr) {
            TreeNode* tmp = new TreeNode(val);
            return tmp;
        }
        helper(root, val);
        return root;

    }
    void helper(TreeNode* root, int val) {
        if(root == nullptr) return ;
        // 该值 大于 根
        if(val > root->val) {
            // 如果右为空，直接插入 返回即可
            if(root->right == nullptr) {
                TreeNode* tmp = new TreeNode(val);
                root->right = tmp;
                // return ;
            }else helper(root->right, val); // 否则 继续遍历右边 因为必须放在右边
        } 
        // 该值 小于 根，需要放在左边
        if(val < root->val) {
            // 左边为空 直接插入返回即可
            if(root->left == nullptr) {
                TreeNode* tmp = new TreeNode(val);
                root->left = tmp;
                // return ;
            }else helper(root->left, val);
        } 
        return ;
    }
};



// 也可以返回 节点
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == NULL) {
            TreeNode* node = new TreeNode(val);
            return node;
        }
        if (root->val > val) root->left = insertIntoBST(root->left, val);
        if (root->val < val) root->right = insertIntoBST(root->right, val);
        return root;
    }
};
```

只要遍历二叉搜索树，找到空节点 插入元素就可以了

## [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/description/)	

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root == nullptr) return nullptr;
        return helper(root, key);
    }
    // 将节点返回 去
    TreeNode* helper(TreeNode* root, int key) {
        if(root == nullptr) return nullptr;
        // 使用前序遍历
        if(root->val == key) {
            // 第一种情况：左右都为空 直接删除 返回空
            if(!root->left && !root->right) {
                delete root;
                return nullptr;
            }
            // 第四种情况 这种情况需要放在前面 左右都不空，将左边节点放在 右边节点的最左侧
            else if(root->left && root->right) {
                // cout << "--" << endl;
                // 寻找 右边 节点的 最左侧节点
                auto cur = root->right;
                // 注意判断条件是 左不为空  并且是循环的
                while(cur->left != nullptr) cur = cur->left;
                cur->left = root->left;
                auto tmp = root->right;
                delete root;
                return tmp;
            }
            // 第二种情况 左不为空 返回 左 即可 需要删除根节点
            else if (root->left){
                // cout << "--" << endl;

                auto tmp = root->left;
                delete root;
                return tmp;
            }
            // 第三种情况 右不为空 返回 右 即可
            else if(root->right) {
                // cout << "--" << endl;

                auto tmp = root->right;
                delete root;
                return tmp;
            }
        }
        // 需要接住 自底向上传过来的 节点
        if(root->val > key) root->left = helper(root->left, key);
        if(root->val < key) root->right = helper(root->right, key);
        return root;
    }
};
```

有以下五种情况：

- 第一种情况：没找到删除的节点，遍历到空节点直接返回了
- 找到删除的节点
  - 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
  - 第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
  - 第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
  - 第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。