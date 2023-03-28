# Day27 回溯 组合

今日内容

● 39. 组合总和

● 40.组合总和II

 ● 131.分割回文串

## [\39. 组合总和](https://leetcode.cn/problems/combination-sum/description/)

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> tmp;
        backtrack(candidates, target, res, tmp, 0);
        return res;
    }
    void backtrack(vector<int>& candidates, int target, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        if(target == 0) {
            res.push_back(tmp);
            return ;
        };
		/*
            for(int i = startIdx; i < candidates.size(); i ++ ) {
                tmp.push_back(candidates[i]);
                // 剪枝
                if(target <= 0) {
                    tmp.pop_back();
                    break;
                }
                backtrack(candidates, target - candidates[i], res, tmp, startIdx);
                tmp.pop_back();
                startIdx ++;    // 每一轮之后 +1
            }
        */
        for(int i = startIdx; i < candidates.size(); i ++ ) {
            tmp.push_back(candidates[i]);
            // 剪枝
            if(target <= 0) {
                tmp.pop_back();
                break;
            }
            // 必须从当前 及之后 选择
            // 区别是当前元素可以继续被选择，所有i
            backtrack(candidates, target - candidates[i], res, tmp, i);
            tmp.pop_back();
            // startIdx ++;    // 每一轮之后 +1
        }
    }
};
```



## [\40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/description/)

给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用 **一次** 。

**注意：**解集不能包含重复的组合。 

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<int> used(candidates.size(), 0);
        sort(candidates.begin(), candidates.end());
        backtrack(candidates, target, used, res, tmp, 0);
        return res;
    }
    void backtrack(vector<int>& candidates, int target, vector<int>& used, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        if(target == 0) {
            res.push_back(tmp);
            return ;
        }

        for(int i = startIdx; i < candidates.size(); i ++ ) {
            // 去重
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if(i > 0 && candidates[i] == candidates[i-1] && !used[i-1]) {
                cout << candidates[i] << ' ' << i << endl;
                continue;
            }
            // 选择
            tmp.push_back(candidates[i]);
            used[i] = 1;
            // 剪枝
            if(target <= 0) {
                tmp.pop_back();
                // 记得也要回溯!!!!!!
                used[i] = 0;
                break;
            }
            backtrack(candidates, target - candidates[i], used, res, tmp, i + 1);
            used[i] = 0;
            tmp.pop_back();
        }
    }
};
```

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> tmp;
        // vector<int> used(candidates.size(), 0);
        sort(candidates.begin(), candidates.end());
        backtrack(candidates, target, res, tmp, 0);
        return res;
    }
    void backtrack(vector<int>& candidates, int target, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        if(target == 0) {
            res.push_back(tmp);
            return ;
        }

        for(int i = startIdx; i < candidates.size(); i ++ ) {
            // 去重
            // 要对同一树层使用过的元素进行跳过
            if(i > startIdx && candidates[i] == candidates[i-1]) {
                // cout << candidates[i] << ' ' << i << endl;
                continue;
            }
            // 选择
            tmp.push_back(candidates[i]);
            // 剪枝
            if(target <= 0) {
                tmp.pop_back();
                break;
            }
            backtrack(candidates, target - candidates[i], res, tmp, i + 1);
            tmp.pop_back();
        }
    }
};
```



## [\131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/description/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

```cpp
class Solution {
public:
    bool helper(string s, int st, int ed) {
        for(int i = st, j = ed; i <= j; i ++, j --) {
            if(s[i] != s[j]) return false;
        }
        return true;
    }
    // 思路：切割，通过切割位置判断
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> tmp;
        backtrack(s, res, tmp, 0);
        return res;
    }
    void backtrack(string s, vector<vector<string>>& res, vector<string>& tmp, int startIdx) {
        // 切割位置已经到最后了
        if(startIdx >= s.size()) {
            res.push_back(tmp);
            return ;
        }
        for(int i = startIdx; i < s.size(); i ++ ) {
            // 判读是否是回文串
            if(helper(s, startIdx, i)) {
                // 获取字串
                string str = s.substr(startIdx, i - startIdx + 1);
                tmp.push_back(str);
            }else continue;

            backtrack(s, res, tmp, i + 1);
            tmp.pop_back();
        }
    }
};
```



