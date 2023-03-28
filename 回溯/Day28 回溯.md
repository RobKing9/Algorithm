# Day28 回溯

● 93.复原IP地址 

● 78.子集 

● 90.子集II

## [\93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> res;
        backtrack(s, res, 0, 0);
        return res;
    }
    void backtrack(string s, vector<string>& res, int startIdx, int cnt) {
        // 结束条件 有三个点
        if(cnt == 3) {
            // 判断最后一段是否合法
            if(helper(s, startIdx, s.size() - 1)) {
                // 合法 加入结果
                res.push_back(s);
            }
            return ;
        }

        for(int i = startIdx; i < s.size(); i ++ ){
            // 合法才会选择，不合法后面就没必要切割了
            if(helper(s, startIdx, i)) {
                // 在字符串指定位置前面插入一个 点
                s.insert(s.begin() + i + 1, '.');
                // 递归，i移动两步是因为 加了一个点
                backtrack(s, res, i + 2, cnt + 1);
                // 撤销 去掉点
                // 删除指定索引开始的指定的字符
                s.erase(s.begin() + i + 1);
            }
        }
    }
    // 判断字符串 st-ed 是否是 0-255的数字
    bool helper(string s, int st, int ed) {
        if(st > ed) return false;
        // 前0
        if(s[st] == '0' && st != ed) return false;
        int num = 0;
        for(int i = st; i <= ed; i ++ ) {
            // 不是数字
            if(s[i] < '0' || s[i] > '9') return false;
            num = num * 10 + (s[i] - '0');
            if(num > 255) return false;
        }
        return true;
    }
};
```

## [\78. 子集](https://leetcode.cn/problems/subsets/description/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        backtrack(nums, res, tmp, 0);
        return res;
    }
    void backtrack(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        // if(tmp.size() <= nums.size()) {
            // 加入树中的 每一个节点
            res.push_back(tmp);
            // return ;
        // }

        for(int i = startIdx; i < nums.size(); i ++ ) {
            tmp.push_back(nums[i]);
            backtrack(nums, res, tmp, i + 1);
            tmp.pop_back();
        }
    }
};
```

## [\90. 子集 II](https://leetcode.cn/problems/subsets-ii/description/)

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

```cpp
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<int> used(nums.size(), 0);
        sort(nums.begin(), nums.end());
        backtrack(nums, res, tmp, 0, used);
        return res;
    }
    
    void backtrack(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, int startIdx, vector<int>& used) {
        res.push_back(tmp);

        for(int i = startIdx; i < nums.size(); i ++ ){
            // 去重
            if(i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;
            tmp.push_back(nums[i]);
            used[i] = 1;
            backtrack(nums, res, tmp, i + 1, used);
            used[i] = 0;
            tmp.pop_back();

        }
    }
};
```

