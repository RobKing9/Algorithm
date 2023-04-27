# Day58 单调栈

● 739. 每日温度 

● 496.下一个更大元素 I  



## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/description/)

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

```golang
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        vector<int> res(temperatures.size(), 0);
        for(int i = temperatures.size() - 1; i >= 0; i -- ) {
            while(!st.empty() && temperatures[i] >= temperatures[st.top()]) st.pop();
            if(st.empty()) res[i] = 0;
            else res[i] = st.top() - i;
            st.push(i);
        }
        return res;
    }
};
```

**要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了**

## [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/description/)

`nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。

给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        for(int i = 0; i < nums1.size(); i ++ ) {
            for(int j = 0; j < nums2.size(); j ++ ) {
                if(nums1[i] == nums2[j]) {
                    bool hasnot = true;
                    for(int k = j; k < nums2.size(); k ++ ) {
                        if(nums2[k] > nums2[j]) {
                            res.push_back(nums2[k]);
                            hasnot = false;
                            break;
                        }
                    }
                    if(hasnot) res.push_back(-1);
                }
            }
        }
        return res;
    }
};
```

