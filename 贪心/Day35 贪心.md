# Day35 贪心

● 435. 无重叠区间 

● 763.划分字母区间 

● 56. 合并区间 

## [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/description/)

给定一个区间的集合 `intervals` ，其中 `intervals[i] = [starti, endi]` 。返回 *需要移除区间的最小数量，使剩余区间互不重叠* 。

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] < b[0];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        int res = 0;
        for(int i = 1; i < intervals.size(); i ++ ) {
            // 如果有相交
            if(intervals[i][0] < intervals[i-1][1]) {
                // 需要取最小的，因为可能后面一个是前一个的子集
                intervals[i][1] = min(intervals[i][1], intervals[i-1][1]);
                res ++;
            }
        }
        return res;
    }
};
```

## [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/description/)

给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        // 记录每个字母最远出现的位置
        int digit[26] = {0};
        for(int i = 0; i < s.size(); i ++ ) {
            digit[s[i] - 'a'] = i;
        }
        // 找到每个区间的左右边界
        // 右边界肯定是最远的
        vector<int> res;
        int left = 0, right = 0;
        for(int i = 0; i < s.size(); i ++ ) {
            // 最远的右边界
            right = max(right, digit[s[i] - 'a']);
            // 到达最远的右边界
            if(i == right) {
                res.push_back(right - left + 1);
                // 更新左边界
                left = i + 1;
            }
        }
        return res;
    }
};
```

![763.划分字母区间](https://code-thinking-1253855093.file.myqcloud.com/pics/20201222191924417.png)

## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/description/)

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        return a[0] < b[0];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> res;
        sort(intervals.begin(), intervals.end(), cmp);
        int st = -1, ed = -1;
        for(int i = 0; i < intervals.size(); i ++ ) {
            // 没有交集
            if(ed < intervals[i][0]) {
                // 加入到结果集 
                // 最开始不应该加入
                if(st != -1) res.push_back({st, ed});
                // 更新 st和ed
                st = intervals[i][0], ed = intervals[i][1];
            }else {
                // 有交集 更新最右端点
                ed = max(ed, intervals[i][1]);
            }
        }
        // 加入最后的区间
        res.push_back({st, ed});
        return res;
    }
};
```

