# Day53 子序列问题

● 1143.最长公共子序列 

● 1035.不相交的线   

● 53. 最大子序和  动态规划 

## [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/description/)

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

```golang
func longestCommonSubsequence(text1 string, text2 string) int {
    size1, size2 := len(text1), len(text2)

    // 动态规划 dp[i][j] 表示以i-1和j-1结尾的 最长公共子序列
    dp := make([][]int, size1 + 1)
    for i := 0; i <= size1; i ++ {
        dp[i] = make([]int, size2 + 1)
    }
    for i := 1; i <= size1; i ++ {
        for j := 1; j <= size2; j ++ {
            if text1[i - 1] == text2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + 1
            }else {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
            }
        }
    }
    return dp[size1][size2]
}

func max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```

## [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/description/)

在两条独立的水平线上按给定的顺序写下 `nums1` 和 `nums2` 中的整数。

现在，可以绘制一些连接两个数字 `nums1[i]` 和 `nums2[j]` 的直线，这些直线需要同时满足满足：

-  `nums1[i] == nums2[j]`
- 且绘制的直线不与任何其他连线（非水平线）相交。

请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int size1 = nums1.size();
        int size2 = nums2.size();
        vector<vector<int>> dp(size1 + 1, vector<int>(size2 + 1, 0));
        for(int i = 1; i <= size1; i ++ ) {
            for(int j = 1; j <= size2; j ++ ) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[size1][size2];
    }
};
```

本题和上一题最长公共子序列的思路是一模一样的

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

```
给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

子数组 是数组中的一个连续部分。
```

动态规划解决

`dp[i]`递推从两个方向推出

- 在 `dp[i-1]`的基础上加上当前值
- 直接从当前值开始，可能之前已经小于0了

注意最后的结果不是 `dp[size - 1]` 最后也有可能重新开始的，结果取所有dp的最大值

```golang
func maxSubArray(nums []int) int {
    size := len(nums)
    if size == 1 {
        return nums[0]
    }
    // dp 表示最大子数组和
    dp := make([]int, size)
    // 初始化
    dp[0] = nums[0]
    // 最大值并不是dp[size - 1] 
    res := dp[0]
    for i := 1; i < size; i ++ {
        // 两个方向推导出来
        // 要么继续上一次的 要么重新开始
        dp[i] = max(dp[i - 1] + nums[i], nums[i])
        if dp[i] > res {
            res = dp[i]
        }
    }
    return res
}

func max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```

贪心方法，用一个sum记录和，每次都要比较最后的结果。当sum小于0的时候，将其置为0

```golang
func maxSubArray(nums []int) int {
    // 贪心
    res := nums[0]

    sum := 0
    for i := 0; i < len(nums); i ++ {
        sum += nums[i]
        res = max(res, sum)
        // 和已经小于0了
        if sum <= 0 {
            sum = 0
        }
    }
    return res
}

func max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```





## 总结

今天的三道题主要是子序列问题，最长公共子序列和不想交的线思路是一模一样的

对于递推公式，我们需要明确可以从哪些方向推出当前的值，这样就可以很快的写出递推公式