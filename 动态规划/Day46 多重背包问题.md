# Day46 多重背包问题

● 139.单词拆分 

● 关于多重背包，你该了解这些！ 

● 背包问题总结篇！  

## [139. 单词拆分](https://leetcode.cn/problems/word-break/description/)

给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。请你判断是否可以利用字典中出现的单词拼接出 `s` 。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

```golang
func wordBreak(s string, wordDict []string) bool {
    //用一个字典树 记录 单词字典
    hashtable := make(map[string]bool)
    for _, v := range wordDict {
        hashtable[v] = true
    }
    m := len(s)
    //dp
    dp := make([]bool, m+1)
    dp[0] = true
    for i:=1; i<=m; i++ {
        for j:=0; j<i; j++ {
            //首先是 那个字符 可以作为开始（已经标为true）
            //其次 这个单词（i-j） 可以在哈希表中找到
            if dp[j] && hashtable[s[j:i]] {
                dp[i] = true
                break
            }
        }
    }
    fmt.Println(dp)
    return dp[m]
}
```

![](https://raw.githubusercontent.com/RobKing9/Blog_Pic/master/202304191419239.png)