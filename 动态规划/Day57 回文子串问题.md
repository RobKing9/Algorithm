# Day57 回文子串问题

● 647. 回文子串  

● 516.最长回文子序列

● 动态规划总结篇

## [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/description/)

给你一个字符串 `s` ，请你统计并返回这个字符串中 **回文子串** 的数目。

**回文字符串** 是正着读和倒过来读一样的字符串。

**子字符串** 是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

```golang
func countSubstrings(s string) int {
    size := len(s)
    // dp
    dp := make([][]bool, size)
    for i := 0; i < size; i ++{
        dp[i] = make([]bool, size)
    }
    res := 0
    for i := size - 1; i >= 0; i -- {
        for j := i; j < size; j ++ {
            if s[i] == s[j] {
                if j - i <= 1 {
                    res ++
                    dp[i][j] = true
                }else if dp[i + 1][j - 1] {
                    res ++
                    dp[i][j] = true
                }
            }
        }
    }
    return res
}
```

## [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)

给你一个字符串 `s` ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。



```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for(int i = 0; i < s.size(); i ++) dp[i][i] = 1;
        for(int i = s.size() - 1; i >= 0; i --) {
            for(int j = i + 1; j < s.size(); j ++ ) {
                if(s[i] == s[j]) {
                    dp[i][j] = dp[i+1][j-1] + 2;
                }else {
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
                }
            }
        }
        return dp[0][s.size()-1];
    }
};
```

