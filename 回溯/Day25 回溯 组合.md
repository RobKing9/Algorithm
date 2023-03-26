# Day25 回溯-组合



今日内容： 

● 216.组合总和III

● 17.电话号码的字母组合

## [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/description/)

找出所有相加之和为 `n` 的 `k` 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 **最多使用一次** 

返回 *所有可能的有效组合的列表* 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> res;
        vector<int> tmp;
        backtrack(k, n, res, tmp, 1);
        return res;
    }
    void backtrack(int k, int n, vector<vector<int>>& res, vector<int>& tmp, int startIdx) {
        // 结束条件
        if(tmp.size() == k) {
            if(n == 0) {
                res.push_back(tmp);
                return ;
            }else return ;
        }
        for(int i = startIdx; i <= 9; i ++ ) {
            // 选择
            tmp.push_back(i);
            // 剪枝优化
            // 如果当前和已经小于等于n了，后面的就没什么意义了，剪枝 返回
            if(n <= 0) {
                tmp.pop_back();
                return ;
            } 
            backtrack(k, n - i, res, tmp, i + 1);
            tmp.pop_back();
        } 
    }
};
```



## [\17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

```cpp
class Solution {
public:
    string table[10] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    vector<string> letterCombinations(string digits) {
        if(digits.size() == 0) return {};
        vector<string> res;
        string tmp;
        backtrack(digits, res, tmp, 0);
        return res;
    }
    void backtrack(string digits, vector<string>& res, string& tmp, int startIdx) {
        // 终止条件
        if(tmp.size() == digits.size()) {
            res.push_back(tmp);
            return;
        }
        // 选择
        for(int i = startIdx; i < digits.size(); i ++ ) {
            // 当前位的 所有选择字符串
            string cur = table[digits[i] - '0'];
            for(int j = 0; j < cur.size(); j ++ ) {
                tmp.push_back(cur[j]);
                backtrack(digits, res, tmp, i + 1);
                tmp.pop_back();
            }
        }
    }
};
```

```cpp
class Solution {
public:
    string table[10] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    vector<string> letterCombinations(string digits) {
        if(digits.size() == 0) return {};
        vector<string> res;
        string tmp;
        backtrack(digits, res, tmp, 0);
        return res;
    }
    void backtrack(string digits, vector<string>& res, string& tmp, int startIdx) {
        // 终止条件
        if(tmp.size() == digits.size()) {
            res.push_back(tmp);
            return;
        }
        // 选择
        // for(int i = startIdx; i < digits.size(); i ++ ) {
            // 将for循环遍历 优化成直接通过 startIdx 取出这一位

            // 当前位的 所有选择字符串
            string cur = table[digits[startIdx] - '0'];
            for(int j = 0; j < cur.size(); j ++ ) {
                tmp.push_back(cur[j]);
                backtrack(digits, res, tmp, startIdx + 1);
                tmp.pop_back();
            }
        // }
    }
};
```

