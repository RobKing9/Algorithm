# Day41 动态规划

● 343. 整数拆分 

● 96.不同的二叉搜索树 

## [343. 整数拆分](https://leetcode.cn/problems/integer-break/description/)

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

```cpp
class Solution {
public:
    int integerBreak(int n) {
        if(n <= 2) return 1;
        if(n == 3) return 2;
        int res = 1;
        if(n % 3 == 1) n -= 4, res *= 4;
        if(n % 3 == 2) n -= 2, res *= 2;
        while(n) {
            n -= 3;
            res *= 3;
        } 
        return res;
    }
};
```

贪心方法



```golang
func integerBreak(n int) int {
    dp := make([]int ,n + 1)
    dp[0] = 0
    dp[1] = 1
    for i := 2; i <= n; i ++ {
        for j := 1; j <= i / 2; j ++ {
            dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]))
        }
    }
    return dp[n]
}

func max(x, y int) int{
    if(x < y) {
        return y
    }
    return x
}
```

动态规划

这个题目类似于剑指offer的 剪绳子问题

## [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/description/)

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

```golang
func numTrees(n int) int {
    // 思路：以i为根节点的总和。左边节点个数 i-1, 右边节点个数 i -j
    dp := make([]int , n + 1)
    // 初始化
    dp[0] = 1
    dp[1] = 1
    // 遍历 2-n
    for i := 2; i <= n; i ++ {
        // 遍历1-i，表示以j为根节点
        for j := 1; j <= i; j ++ {
            // j-1 是以j为根节点 左边的结点数量
            // i -j 是右边的结点数量
            dp[i] += dp[j - 1] * dp[i - j]
        }
    }
    return dp[n]
}
```

从递归公式上来讲，dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量] 中以j为头结点左子树节点数量为0，也需要dp[以j为头结点左子树节点数量] = 1， 否则乘法的结果就都变成0了。所以dp[0] = 1