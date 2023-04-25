# Day54 子序列问题

● 392.判断子序列 

● 115.不同的子序列  

## [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/description/)

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

```golang
func isSubsequence(s string, t string) bool {
    size1, size2 := len(s), len(t)
    // dp[i][j] 表示以下标i-1为结尾的字符串s，和以下标j-1为结尾的字符串t，相同子序列的长度为dp[i][j]
    dp := make([][]int, size1 + 1)
    for i := 0; i <= size1; i ++ {
        dp[i] = make([]int, size2 + 1)
    }
    for i := 1; i <= size1; i ++ {
        for j := 1; j <= size2; j ++ {
            // 相等的时候 长度 +1
            if s[i - 1] == t[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + 1
            }else {
                // 相当于t要删除 继续匹配
                dp[i][j] = dp[i][j - 1]
            }
        }
    }
    // 最后的长度和s的长度相等
    if dp[size1][size2] == size1 {
        return true
    }
    return false
}
```

## [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/description/)

给你两个字符串 `s` 和 `t` ，统计并返回在 `s` 的 **子序列** 中 `t` 出现的个数。

题目数据保证答案符合 32 位带符号整数范围。

```golang
func numDistinct(s string, t string) int {
    size1, size2 := len(s), len(t)
    // dp[i][j] 表示以i-1结尾的s 和 以j-1结尾的t，出现的个数
    dp := make([][]int, size1 + 1)
    for i := 0; i <= size1; i ++ {
        dp[i] = make([]int, size2 + 1)
    }
    // 初始化 t为空串，次数为1
    for i := 0; i < size1; i ++ {
        dp[i][0] = 1
    }
    // s为空串，次数为0
    for i := 1; i < size2; i ++ {
        dp[0][i] = 0
    }
    for i := 1; i <= size1; i ++ {
        for j := 1; j <= size2; j ++ {
            if s[i - 1] == t[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
            }else {
                dp[i][j] = dp[i - 1][j]
            }
        }
    }
    return dp[size1][size2]
}
```

