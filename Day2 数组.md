## Day2 | 977.有序数组的平方 | 209.长度最小的子数组 | 59.螺旋矩阵II



## [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> res(nums.size(), 0);
        int left = 0, right = nums.size() - 1, k = right;
        while(left <= right) {
            if(nums[right] * nums[right] > nums[left] * nums[left]) {
                res[k--] = nums[right] * nums[right];
                right --;
            }else {
                res[k --] = nums[left] * nums[left];
                left ++;
            }
        }
        return res;
    }
};
```

理解：之所以可以用双指针，必须从大的开始添加到结果中



## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int slow = 0, sum = 0, res = INT32_MAX;
        for(int fast = 0; fast < nums.size(); fast ++ ) {
            sum += nums[fast];
            // 循环
            while(sum >= target) {
                res = min(res, fast - slow + 1);
                sum -= nums[slow];
                slow ++;
            }
        }
        return res == INT32_MAX ? 0 : res;
    }
};
```

理解：因为是连续子数组，所以不需要排序。INT32_MAX最大值

## [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix`

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));
        // 当前位置
        int x = 0, y = 0;
        // 方向数组
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        // 使用数组
        for(int k = 1, x = 0, y = 0, d = 0; k <= n * n; k ++) {
            res[x][y] = k;
            // 下一个位置的坐标
            int a = x + dx[d], b = y + dy[d];
            // 改变方向
            if(a < 0 || a >= n || b < 0 || b >= n || res[a][b]) {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }
        return res;
    }
};
```

## 数组总结

数组用到的算法主要是二分，双指针，滑动窗口。

