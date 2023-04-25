# Day56 编辑距离问题

● 583. 两个字符串的删除操作 

● 72. 编辑距离 

● 编辑距离总结篇   

```golang
func minDistance(word1 string, word2 string) int {
    size1, size2 := len(word1), len(word2)
    // dp[i][j] 表示word1以i-1结尾，Word2以j-1结尾，相同所需的最小步数
    dp := make([][]int, size1 + 1)
    for i := 0; i <= size1; i ++ {
        dp[i] = make([]int, size2 + 1)
    }
    // 初始化
    for i := 0; i <= size1; i ++ {
        dp[i][0] = i 
    }
    for i := 0; i <= size2; i ++ {
        dp[0][i] = i
    } 
    for i := 1; i <= size1; i ++ {
        for j := 1; j <= size2; j ++ {
            if word1[i - 1] == word2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1]
            }else {
                // 不相同需要一步操作
                dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1)
            }
        }
    }
    return dp[size1][size2]
}   

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

## [72. 编辑距离](https://leetcode.cn/problems/edit-distance/description/)

给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

```golang
func minDistance(word1 string, word2 string) int {
    size1, size2 := len(word1), len(word2)
    // dp[i][j] 表示word1以i-1结尾，Word2以j-1结尾，相同所需的最小步数
    dp := make([][]int, size1 + 1)
    for i := 0; i <= size1; i ++ {
        dp[i] = make([]int, size2 + 1)
    }
    // 初始化
    for i := 0; i <= size1; i ++ {
        dp[i][0] = i 
    }
    for i := 0; i <= size2; i ++ {
        dp[0][i] = i
    } 
    for i := 1; i <= size1; i ++ {
        for j := 1; j <= size2; j ++ {
            if word1[i - 1] == word2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1]
            }else {
                // 不相同需要一步操作
                dp[i][j] = min(dp[i - 1][j] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1))
            }
        }
    }
    return dp[size1][size2]
}   

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

**word2添加一个元素，相当于word1删除一个元素**
