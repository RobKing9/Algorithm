# Day21 二叉树

今日内容 

● 530.二叉搜索树的最小绝对差 

● 501.二叉搜索树中的众数 

● 236. 二叉树的最近公共祖先  

## [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

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
    // 初始值 res 尽可能大，因为求的是最小值
    // 需要赋值一个 较小的值，这样第一次减的时候会特别大而不影响结果
    int res = 10e5, pre = -1;
    int getMinimumDifference(TreeNode* root) {
         getMin(root);
         return res;
    }
    void getMin(TreeNode* root) {
        if(root == nullptr) return ;
        getMin(root->left);
        // 处理
        if(pre != -1) res = min(res, root->val - pre);
        pre = root->val;
        getMin(root->right); 
    }
};
```

以节点 保存 节点

**在二叉树中通过两个前后指针作比较**

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
    int res = 10e5;
    TreeNode* pre = nullptr;
    int getMinimumDifference(TreeNode* root) {
         getMin(root);
         return res;
    }
    void getMin(TreeNode* root) {
        if(root == nullptr) return ;
        getMin(root->left);
        // 处理
        if(pre != nullptr) res = min(res, root->val - pre->val);
        pre = root;
        getMin(root->right); 
    }
};
```



## [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

给你一个含重复值的二叉搜索树（BST）的根节点 `root` ，找出并返回 BST 中的所有 [众数](https://baike.baidu.com/item/众数/44796)（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 **任意顺序** 返回。

假定 BST 满足如下定义：

- 结点左子树中所含节点的值 **小于等于** 当前节点的值
- 结点右子树中所含节点的值 **大于等于** 当前节点的值
- 左子树和右子树都是二叉搜索树

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
    // cnt 计数，maxCne 当前出现频率最大
    int cnt = 0, maxCnt = 0;
    TreeNode* pre = nullptr;
    vector<int> findMode(TreeNode* root) {
        if(root == nullptr) return {};
        // 思路 cnt 来计数   记完之后 和 maxCnt比较
        // 如果等于加到数组中，如果大于，清空数组，加入进去（因为这个时候最大已经更新）
        vector<int> res;
        helper(root, res);
        return res;
    }
    void helper(TreeNode* root, vector<int>& res) {
        if(root == nullptr) return ;
        helper(root->left, res);
        // 处理
        if(pre != nullptr) {
            if(root->val == pre->val) cnt ++;
            else cnt = 1;
        }else cnt = 1;
        pre = root;
        if(cnt == maxCnt) res.push_back(pre->val);
        else if(cnt > maxCnt) {
            // 大于 最大频率 需要更新
            maxCnt = cnt;
            // 之前记录的都无效了 清楚
            res.clear();
            // 加入新的最大值
            res.push_back(pre->val);
        }
        helper(root->right, res);
    }
};
```

频率count 大于 maxCount的时候，不仅要更新maxCount，而且要清空结果集（以下代码为result数组），因为结果集之前的元素都失效了。



## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。

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
    // 条件是 两个不相等，并且整棵树 所有结点的值 都不相等
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // 使用后序遍历，先遍历左右两边，然后中间处理
        // 如果遇到了 p或者q 直接返回 p或者q 否则就是返回空
        // 处理的过程就是 看左右返回的值 
        // 如果都不为空的话，那么左右都找到了，返回root 其实就是他们的祖先
        // 如果只有一边不为空的话 说明只有这一边找到了，返回这一边，然后继续去左右遍历

        // 另外如果两个互为 最近祖先的话，将最上面那个返回，下面就不需要遍历了
        // 因为最终返回的只有一边，刚好符合条件
        
        if(root == nullptr) return nullptr;
        // 如果遇到了 p或者q 直接返回 p或者q 
        if(root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        // 都不为空  如果是在最下面找到的 依然会一步一步返回上去
        if(left && right) return root;
        if(left) return left;   // 左不为空 返回左
        if(right) return right;
        // 都没找到 不满足条件
        return nullptr;
    }
};
```

这道题目的看代码比较简单，而且好像也挺好理解的，但是如果把每一个细节理解到位，还是不容易的。

主要思考如下几点：

- 如何从底向上遍历？
- 遍历整棵树，还是遍历局部树？
- 如何把结果传到根节点的？