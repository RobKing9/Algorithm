# Day52 最长问题

## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

```golang
func lengthOfLIS(nums []int) int {
    size := len(nums)
    if size == 1 {
        return 1
    }
    // 动态规划 dp[i] 表示以nums[i] 结尾的最长
    dp := make([]int, size)
    // 初始化 开始都是1
    for i := 0; i < size; i ++ {
        dp[i] = 1
    }
    // 结果取 dp中最大值
    res := 1
    for i := 0; i < size; i ++ {
        for j := 0; j < i; j ++ {
            // 当前 大于 之前的 序列+1
            if nums[i] > nums[j] {
                // 取之前 dp[j] 的最大值
                dp[i] = max(dp[i], dp[j] + 1)
            }
        }
        // 取dp的最大值
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

## [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/description/)

给定一个未经排序的整数数组，找到最长且 **连续递增的子序列**，并返回该序列的长度。

**连续递增的子序列** 可以由两个下标 `l` 和 `r`（`l < r`）确定，如果对于每个 `l <= i < r`，都有 `nums[i] < nums[i + 1]` ，那么子序列 `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` 就是连续递增子序列。

```golang
func findLengthOfLCIS(nums []int) int {
    size := len(nums)
    if size == 1 {
        return 1
    }
    // 动态规划 dp[i] 表示以nums[i] 结尾的最长
    dp := make([]int, size)
    // 初始化 开始都是1
    for i := 0; i < size; i ++ {
        dp[i] = 1
    }
    // 结果取 dp中最大值
    res := 1
    for i := 1; i < size; i ++ {
            // 当前 大于 之前的 序列+1
        if nums[i] > nums[i - 1] {
            // 取之前 dp[j] 的最大值
            dp[i] = dp[i - 1] + 1
        }
        // 取dp的最大值
        if dp[i] > res {
            res = dp[i]
        }
    }
    return res
}
```

## [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)

给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 **公共的** 、长度最长的子数组的长度* 。

```golang
func findLength(nums1 []int, nums2 []int) int {
    // dp[i][j] 表示 以nums的结尾下标i-1和nums2的结尾下标j-1 最长公共子数组
    dp := make([][]int, len(nums1) + 1)
    for i := 0; i <= len(nums1); i ++ {
        dp[i] = make([]int, len(nums2) + 1)
    }
    res := 0
    for i := 1; i <= len(nums1); i ++ {
        for j := 1; j <= len(nums2); j ++ {
            // 根据dp[i][j]的定义，dp[i][j]的状态只能由dp[i - 1][j - 1]推导出来。
            if nums1[i - 1] == nums2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + 1
            }
            if dp[i][j] > res {
                res = dp[i][j]
            }
        }
    }
    return res
}
```

