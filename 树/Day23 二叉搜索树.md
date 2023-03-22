# Day23 二叉搜索树

 今日内容：

● 669. 修剪二叉搜索树 

● 108.将有序数组转换为二叉搜索树 

● 538.把二叉搜索树转换为累加树 

 ● 总结篇 

## [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/description/)

给你二叉搜索树的根节点 `root` ，同时给定最小边界`low` 和最大边界 `high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树 **不应该** 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 **唯一的答案** 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        // 递归结束
        if(root == nullptr) return nullptr;
        // 小于 low, 说明当前节点的左边都没用了，因为都小于
        // 右边 继续范围内寻找，可能有符合的，也有不符合的. 然后将其返回给根节点(挂在左边)
        if(root->val < low) {
            auto right = trimBST(root->right, low, high);
            return right;
        }
        // 大于 high 类似的做法
        if(root->val > high) {
            auto left = trimBST(root->left, low, high);
            return left;
        }
        // 递归左右, 将返回值作为 root的左和右子树
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};
```

有返回值，可以通过递归函数的返回值来移除节点

## [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。

**高度平衡** 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.size() == 0) return nullptr;
        int mid = nums.size() >> 1;
        TreeNode* root = new TreeNode(nums[mid]);
        vector<int> left, right;
        for(int i = 0; i < mid; i ++ ) left.push_back(nums[i]);
        for(int i = mid + 1; i < nums.size(); i ++ ) right.push_back(nums[i]);
        // vector<int> left(nums.begin(), nums.begin() + mid);
        // vector<int> right(nums.begin() + mid + 1, nums.end());
        // cout << left[mid - 1] << ' ' << right[mid +1];
        root->left = sortedArrayToBST(left);
        root->right = sortedArrayToBST(right);
        return root;
    }
};
```

不断中间分割，然后递归处理左区间，右区间

## [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)

给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

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
    TreeNode* convertBST(TreeNode* root) {
        if(root == nullptr) return nullptr;
        int addNum = 0;
        dfs(root, addNum);
        return root;
    }
    void dfs(TreeNode* root, int &addNum) {
        if(root == nullptr) return ;
        dfs(root->right, addNum);
        addNum += root->val;
        root->val = addNum;
        dfs(root->left, addNum);
    }
};
```

**右中左来遍历二叉树**，依然需要一个pre指针记录当前遍历节点cur的前一个节点，这样才方便做累加。