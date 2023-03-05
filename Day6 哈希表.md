#### 

# Day6  | 242.有效的字母异位词  | 349. 两个数组的交集 

# 202.快乐数 |  1. 两数之和   





## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;

        const int size = s.size();
        int res[26] = {0};
        for(int i = 0; i < size; i ++ ){
            res[s[i] - 'a'] ++;
        }
        for(int i = 0; i < size; i ++ ) res[t[i] - 'a'] --;

        for(int i = 0; i < 26; i ++ ) {
            if(res[i] != 0) return false;
        } 
        return true;
    }
};
```

## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> res;
        // 对集合1去重
        unordered_set<int> nums1_set(nums1.begin(), nums1.end());
        for(auto num : nums2) {
            // 在集合2中存在
            if(nums1_set.find(num) != nums1_set.end()) {
                res.insert(num);
            }
        }
        // 强制转化
        return vector<int>(res.begin(), res.end());
    }
};
```

理解：unordered_set 是不考虑顺序的，底层实现是哈希表，时间复杂度是O(1);

## [202. 快乐数](https://leetcode.cn/problems/happy-number/)

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
- 如果这个过程 结果为 1，那么这个数就是快乐数。
- 如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

```cpp
class Solution {
public:
    // 获取各个位的平方和
    int getSum(int n) {
        int sum = 0;
        while(n) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> flag;
        while(1) {
            flag.insert(n);
            int sum = getSum(n);
            if(sum == 1) return true;
            else if(flag.find(sum) != flag.end()) return false;
            else n = sum;
        }
        return false;
        
    }
};	
```

理解：学会了用循环获取各个位上的平方和。使用set可以去重

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> res;
        for(int i = 0; i < nums.size(); i ++) {
            if(res.find(target - nums[i]) != res.end()) {
                return vector<int>{i, res[target - nums[i]]};
            }
            res[nums[i]] = i;
             //  auto iter = map.find(target - nums[i]); 
            // if(iter != map.end()) {
            //     return {iter->second, i};
            // }
            // // 如果没找到匹配对，就把访问过的元素和下标加入到map中
            // map.insert(pair<int, int>(nums[i], i)); 
        }
        return {};
    }
};
```

