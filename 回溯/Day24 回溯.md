# Day24 回溯

今日内容：

● 理论基础 

● 77. 组合  

![](https://raw.githubusercontent.com/RobKing9/Blog_Pic/master/20230324000129.png)

## 理论基础

```cpp
回溯算法模板框架如下：

void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

因为回溯算法需要的参数可不像二叉树递归的时候那么容易一次性确定下来，所以一般是先写逻辑，然后需要什么参数，就填什么参数。

for循环就是遍历集合区间，可以理解一个节点有多少个孩子，这个for循环就执行多少次。

**for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历**，这样就把这棵树全遍历完了，一般来说，搜索叶子节点就是找的其中一个结果了

回溯法，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等



## [77. 组合](https://leetcode.cn/problems/combinations/description/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> tmp;
        backtrack(n, k, res, tmp, 1);
        return res;
    }
    // 用startIdx 保存开始的位置
    void backtrack(int n, int k, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        // 终止条件
        if(tmp.size() == k) {
            // cout << tmp[0] << ' ' << tmp[1] << endl;
            res.push_back(tmp);
            return ;
        }
        // 选择
        // for(int i = startIdx; i <= n; i ++ ) {
        // 优化
         // k 是需要的总长度，k - tmp.size() 表示还需要的长度
        // n - (k - tmp.size()) + 1 之后的下标就没什么意义了, 因为长度已经不够了, 剪枝
        for(int i = startIdx; i <= n - (k - tmp.size()) + 1; i ++ ) {
            tmp.push_back(i);
            // 回溯  注意这里的回溯是 i + 1 而不是 startIdx + 1
            // 表示每一层循环 不然如果是2的话，在这层循环中 startIdx 依然是 1， 那就会出现 [2, 2]
            backtrack(n, k, res, tmp, i + 1);
            // 撤销选择
            tmp.pop_back();
        }
    }
};
```

看一下优化过程如下：

1. 已经选择的元素个数：path.size();
2. 还需要的元素个数为: k - path.size();
3. 在集合n中至多要从该起始位置 : n - (k - path.size()) + 1，开始遍历

为什么有个+1呢，因为包括起始位置，我们要是一个左闭的集合。

举个例子，n = 4，k = 3， 目前已经选取的元素为0（path.size为0），n - (k - 0) + 1 即 4 - ( 3 - 0) + 1 = 2。

从2开始搜索都是合理的，可以是组合[2, 3, 4]。