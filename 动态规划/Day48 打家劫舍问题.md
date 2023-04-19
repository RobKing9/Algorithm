# Day48 打家劫舍问题

● 198.打家劫舍 

● 213.打家劫舍II  

● 337.打家劫舍III

## [198. 打家劫舍](https://leetcode.cn/problems/house-robber/description/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if(size == 1) return nums[0];
        // 表示偷窃的 最大金额
        vector<int> dp(size + 1, 0);
        // 初始化
        dp[0] = nums[0], dp[1] = max(dp[0], nums[1]);
        // 遍历
        for(int i = 2; i < nums.size(); i ++ ) {
            // 当前 偷或者不偷
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[size - 1];
    }   
};
```

## [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/description/)

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 **围成一圈** ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警** 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **在不触动警报装置的情况下** ，今晚能够偷窃到的最高金额。



```golang
func rob(nums []int) int {
    size := len(nums)
    if size == 1 {
        return nums[0]
    }
    // 第一个偷或者不偷
    return max(robNil(nums[1:]), robNil(nums[:(size - 1)]))
}

func robNil(nums []int) int {
    size := len(nums)
    if size == 1 {
        return nums[0]
    }
    dp := make([]int, size + 1)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    for i := 2; i < size; i ++ {
        dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
    }
    return dp[size - 1]
}

func max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```

## [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/)

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

 

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func rob(root *TreeNode) int {
    if root == nil {
        return -1
    }
    dp := myRob(root)
    return max(dp[0], dp[1])
}

func myRob(root *TreeNode) []int {
    // dp[0], dp[1] 当前节点 不偷的最大值和偷的最大值
    // 递归结束
    if root == nil {
        return []int{0, 0}
    }
    // 后序遍历
    leftDp := myRob(root.Left)
    rightDp := myRob(root.Right)
    // 当前节点 偷, 所以左右节点都不能偷
    val1 := root.Val + leftDp[0] + rightDp[0]
    // 当前节点 不偷，看左右节点哪种方式最大
    val2 := max(leftDp[0], leftDp[1]) + max(rightDp[0], rightDp[1])
    // 返回两种情况
    return []int{val2, val1}
}

func max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```

