

## [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/description/)

给定一个循环数组 `nums` （ `nums[nums.length - 1]` 的下一个元素是 `nums[0]` ），返回 *`nums` 中每个元素的 **下一个更大元素*** 。

数字 `x` 的 **下一个更大的元素** 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1` 。

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> res;
        vector<int> tmp = nums;
        for(int i = 0; i < nums.size(); i ++ ) tmp.push_back(nums[i]);
        for(int i = 0; i < nums.size(); i ++) {
            bool high = false;
            for(int j = i + 1; j < tmp.size(); j ++ ) {
                if(tmp[j] > tmp[i]) {
                    res.push_back(tmp[j]);
                    high = true;
                    break;
                }
            }
            if(!high) res.push_back(-1);
        }
        return res;
    }
};
```

