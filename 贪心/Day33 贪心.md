# Day33 贪心

● 1005.K次取反后最大化的数组和 

● 134. 加油站

● 135. 分发糖果  

## [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/description/)

给你一个整数数组 `nums` 和一个整数 `k` ，按以下方法修改该数组：

- 选择某个下标 `i` 并将 `nums[i]` 替换为 `-nums[i]` 。

重复这个过程恰好 `k` 次。可以多次选择同一个下标 `i` 。

以这种方式修改数组后，返回数组 **可能的最大和** 。

```cpp
class Solution {
public:
    // 按照绝对值从大到小排序
    static bool cmp (int a, int b) {
        return abs(a) > abs(b);
    }
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp);
        for(int i = 0; i < nums.size(); i ++ ) {
            if(nums[i] < 0 && k > 0) {
                nums[i] *= -1;
                k --;
            }
        }
        // 最后k为整数，改变最后一个即可
        if(k%2) nums[nums.size() - 1] *= -1;
        int res = 0;
        for(int num : nums) res += num;
        return res;
    }
};
```

贪心的思路，局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大。



## [134. 加油站](https://leetcode.cn/problems/gas-station/description/)

在一条环路上有 `n` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 `gas` 和 `cost` ，如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 `-1` 。如果存在解，则 **保证** 它是 **唯一** 的。

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        // 思路：判断 积累的油是否可以到达当前位置
        int cur = 0;    // 当前的油
        int total = 0;  // 总的油，如果小于0，那肯定不行
        int start = 0;  // 开始位置，即最后的结果
        for(int i = 0; i < gas.size(); i ++ ) {
            cur += (gas[i] - cost[i]);
            total += (gas[i] - cost[i]);
            if(cur < 0) {
                // 到当前位置油不够了，从当前位置的下一个位置开始
                start = i + 1;
                cur = 0;    // 置为0
            }
        }
        if(total < 0) return -1;
        return start;
    }
};
```

**那么局部最优：当前累加rest[i]的和curSum一旦小于0，起始位置至少要是i+1，因为从i之前开始一定不行。全局最优：找到可以跑一圈的起始位置**。

## [135. 分发糖果](https://leetcode.cn/problems/candy/description/)

`n` 个孩子站成一排。给你一个整数数组 `ratings` 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

- 每个孩子至少分配到 `1` 个糖果。
- 相邻两个孩子评分更高的孩子会获得更多的糖果。

请你给每个孩子分发糖果，计算并返回需要准备的 **最少糖果数目** 。

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> cnt(ratings.size(), 1);
        // for(int i = 0; i < ratings.size(); i ++ ) cout << cnt[i] << endl;
        // 从左往右遍历，右边比左边大的话，需要大1
        for(int i = 1; i < ratings.size(); i ++ ) {
            if(ratings[i] > ratings[i-1]) cnt[i] = cnt[i-1]+1;
        }
        // 从右往左遍历，左边比右边大的话，需要大1.而且要取最大值
        for(int i = ratings.size() - 2; i >= 0; i -- ) {
            if(ratings[i] > ratings[i+1]) cnt[i] = max(cnt[i+1] + 1, cnt[i]);
        }
        int res = 0;
        for(auto i : cnt) res += i;
        return res;
    }   
};
```

本题采用了两次贪心的策略：

- 一次是从左到右遍历，只比较右边孩子评分比左边大的情况。
- 一次是从右到左遍历，只比较左边孩子评分比右边大的情况。

这样从局部最优推出了全局最优，即：相邻的孩子中，评分高的孩子获得更多的糖果。