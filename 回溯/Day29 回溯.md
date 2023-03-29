# Day29 回溯

## [491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/description/)

给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

```cpp
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        backtrack(nums, res, tmp, 0);
        return res;
    }
    void backtrack(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        if(tmp.size() >= 2) {
            res.push_back(tmp);
        }
        if(tmp.size() == nums.size()) return ;
        unordered_set<int> set;
        for(int i = startIdx; i < nums.size(); i ++ ) {
            // 去重
            // 1、不为空，而且当前值小于 tmp的最后一个值
            // 2、同一层已经出现过了
            if((!tmp.empty() && nums[i] < tmp.back()) || set.count(nums[i])) continue;
            set.insert(nums[i]);
            tmp.push_back(nums[i]);
            // 回溯递归
            backtrack(nums, res, tmp, i + 1);
            tmp.pop_back();
        }
    }
};
```

**这也是需要注意的点，`unordered_set<int> uset;` 是记录本层元素是否重复使用，新的一层uset都会重新定义（清空），所以要知道uset只负责本层！**

## [46. 全排列](https://leetcode.cn/problems/permutations/description/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<int> used(nums.size(), 0);
        backtrack(nums, res, tmp, used);
        return res;
    }
    void backtrack(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, vector<int>& used) {
        if(tmp.size() == nums.size()) {
            res.emplace_back(tmp);
            return ;
        }
        for(int i = 0; i < nums.size(); i ++ ){
            if(!used[i]) tmp.emplace_back(nums[i]);
            else continue;
            used[i] = 1;
            backtrack(nums, res, tmp, used);
            tmp.pop_back();
            used[i] = 0;
        }
    }
};
```



## [\47. 全排列 II](https://leetcode.cn/problems/permutations-ii/description/)

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<int> used(nums.size(), 0);
        sort(nums.begin(), nums.end());
        backtrack(nums, res, tmp, used);
        return res;
    }
    void backtrack(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, vector<int>& used) {
        if(tmp.size() == nums.size()) {
            res.emplace_back(tmp);
            return ;
        }
        for(int i = 0; i < nums.size(); i ++ ){
            if(i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;
            if(!used[i]) tmp.emplace_back(nums[i]);
            else continue;
            used[i] = 1;
            backtrack(nums, res, tmp, used);
            tmp.pop_back();
            used[i] = 0;
        }
    }
};
```

**对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高！**