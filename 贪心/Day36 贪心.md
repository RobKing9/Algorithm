# Day36 贪心

● 738.单调递增的数字 

● 968.监控二叉树 

● 总结 

## [738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/description/)

当且仅当每个相邻位数上的数字 `x` 和 `y` 满足 `x <= y` 时，我们称这个整数是**单调递增**的。

给定一个整数 `n` ，返回 *小于或等于 `n` 的最大数字，且数字呈 **单调递增*** 。

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string tmp = to_string(n);
        int start = tmp.size(); // start及之后都要变成9
        // 从后遍历
        for(int i = tmp.size() - 1; i > 0; i --) {	
            // 如果不是递增了，那么 需要改变 前一个变小，之后的全变成9
            if(tmp[i-1] > tmp[i]) {
                start = i;
                tmp[i-1] --;
            }
        }
        for(int i = start; i < tmp.size(); i ++ ) tmp[i] = '9';
        return stoi(tmp);
    }
};
```

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/%E8%B4%AA%E5%BF%83%E6%80%BB%E7%BB%93water.png)