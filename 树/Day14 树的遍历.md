# Day14 | 树的遍历

前序递归遍历

```cpp
struct TreeNode {
	int val;
	TreeNode * left;
	TreeNode * right;
	TreeNode(): val(0), left(nullptr), right(nullptr);
	TreeNode(int val): val(val), left(NULL), right(NULL);
	TreeNode(int val, TreeNode * left, TreeNode * right): val(val), left(left), right(right);
};

class Solution {
public:
	vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr) return {};
        vector<int> res;
        dfs(root, res);
        return res;
    }    
    void dfs(TreeNode * root, vector<int> &res) {
        if(root == nullptr) return;
        res.push_back(root->val);
        dfs(root->left, res);
        dfs(root->right, res);
        return;
    }
};
```

前序非递归

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
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr) return {};
        vector<int> res;
        stack<TreeNode *> st;
        st.push(root);
        while(!st.empty()) {
            TreeNode * top = st.top();
            res.push_back(top->val);
            st.pop();
            if(top->right) st.push(top->right); // 右边先入栈，这样就后出来
            if(top->left) st.push(top->left);
        }
        return res;
    }
};
```

后序非递归

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
    vector<int> postorderTraversal(TreeNode* root) {
        // 后序顺序为 左右中  可以通过和前序一样的方法到达 中右左，然后翻转即可变成 左右中
        if(root == nullptr) return {};
        vector<int> res;
        stack<TreeNode *> st;
        st.push(root);
        while(!st.empty()) {
            TreeNode * top = st.top();
            st.pop();
            res.push_back(top->val);
            if(top->left) st.push(top->left);
            if(top->right) st.push(top->right);
        }
        // 记得一定要翻转
        reverse(res.begin(), res.end());
        return res;
    }
};
```

中序非递归遍历

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
    vector<int> inorderTraversal(TreeNode* root) {
        if(root == nullptr) return {};
        stack<TreeNode *> st;
        vector<int> res;
        TreeNode * cur = root;
        // 循环条件是或的关系 一方面是到达了叶子结点，另一方面是左边处理完了，这个时候栈为空但是右边还没处理
        while(cur != nullptr || !st.empty()) {
            if(cur != nullptr) {
                // 没有到结尾
                st.push(cur);
                cur = cur->left;	// 一直往左走（左）
            }else {
                cur = st.top();	
                st.pop();
                res.push_back(cur->val);	// 中间的（中）
                cur = cur->right; 	// 右边走（右）
            }
        }
        return res;
    }
};
```

