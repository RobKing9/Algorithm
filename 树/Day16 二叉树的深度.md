# 二叉树 | 104.二叉树的最大深度  559.n叉树的最大深度

# 111.二叉树的最小深度 |  222.完全二叉树的节点个数



## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)

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
    int maxDepth(TreeNode* root) {
        // 到根节点，高度为0
        if(root == nullptr) return 0;
        // 左右中 后序遍历
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        int res = 1 + max(left, right);
        return res;
    }
};
```

## [559. N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/description/)

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

N 叉树输入按层序遍历序列化表示，每组子节点由空值分隔（请参见示例）。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr) return 0;
        int size = root->children.size();
        int maxv = 0;
        for(int i = 0; i < size; i ++ ) {
            // 左右子树中 的最大值
            maxv = max(maxv, maxDepth(root->children[i]));
        }
        // 包括自己 +1
        return maxv + 1;
    }
};
```

非递归

```cpp
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr) return 0;
        queue<Node *> qu;
        int res = 0;
        qu.push(root);
        while(!qu.empty()) {
            res ++;
            int size = qu.size();
            while(size --) {
                auto top = qu.front();
                qu.pop();
                for(int i = 0; i < top->children.size(); i ++ ) {
                    if(top->children[i]) qu.push(top->children[i]);
                }
            }
        }
        return res;
    }
};
```

## [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/description/)

给你一棵 **完全二叉树** 的根节点 `root` ，求出该树的节点个数。

[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin) 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 `h` 层，则该层包含 `1~ 2h` 个节点。

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
    int countNodes(TreeNode* root) {
        // 左右子树所有节点数之和
        if(root == nullptr) return 0;
        int left = countNodes(root->left);
        int right = countNodes(root->right);
        int res = left + right;
        // 还要加上自身
        return res + 1;
    }
};
```

