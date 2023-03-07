# Day7 | 454.四数相加II |  383. 赎金信 | 15. 三数之和  | 18. 四数之和



## [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

- 0 <= i, j, k, l < n

- nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        // key: a + b value: a + b 出现的次数
        unordered_map <int, int> map;
        for (int a : nums1) {
            for (int b : nums2) {
                map[a + b]++;
            }
        }
        int count = 0;
        for (int c : nums3) {
            for (int d : nums4) {
                if (map.find(0-(c+d)) != map.end()) {
                    count += map[0-(c+d)];
                }
            }
        }
        return count;
    }
};
```

## [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> m;
        for(int i = 0; i < magazine.size(); i ++ ) {
            m[magazine[i]] ++;
        }
        for(int i = 0; i < ransomNote.size(); i ++ ) {
            m[ransomNote[i]] --;
        }
        for(auto it = m.begin(); it != m.end(); it ++ ) {
            if(it->second < 0) return false;
        }
        return true;
    }
};
```

这一题直接用数组可能会更好一点



## [15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请

你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int i = 0;
        for(int i = 0; i < nums.size() - 2; i ++) {
            if(nums[i] > 0) return res;
            // 去重
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            int j = i + 1, k = nums.size() - 1;
            while(j < k) {
                if(nums[i] + nums[j] + nums[k] == 0) {
                    res.push_back({nums[i], nums[j], nums[k]});
                    // 去重
                    while(j < k && nums[j] == nums[j + 1]) j ++;
                    while(j < k && nums[k] == nums[k - 1]) k --;
                    // 双指针同时收缩
                    j ++;
                    k --;
                }else if(nums[i] + nums[j] + nums[k] < 0) {
                    j ++;
                }else {
                    k --;
                }
            }
        }
        return res;
        
    }
};
```

理解：需要注意的就是去重的逻辑

## [18. 四数之和](https://leetcode.cn/problems/4sum/)

给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

- 0 <= a, b, c, d < n

- a、b、c 和 d 互不相同
- nums[a] + nums[b] + nums[c] + nums[d] == target

你可以按 任意顺序 返回答案 。

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for(int a = 0; a < nums.size(); a ++ ) {
            // 剪枝 需要大于等于0，防止target为负数
            if(nums[a] > target && nums[a] >= 0) return res;
            // 去重
            if(a > 0 && nums[a] == nums[a - 1]) {
                continue;
            }
            for(int b = a + 1; b < nums.size(); b ++ ) {
                // 剪枝
                if(nums[a] + nums[b] > target && nums[a] + nums[b] >= 0) break;
                // 去重
                if(b > a + 1 && nums[b] == nums[b - 1]) continue;

                int c = b + 1, d = nums.size() - 1;
                while(c < d) {
                    // 防止越界
                    if((long)nums[a] + nums[b] + nums[c] + nums[d] == target) {
                        res.push_back({nums[a], nums[b], nums[c], nums[d]});
                        // 去重
                        while(c < d && nums[c] == nums[c + 1]) c ++;
                        while(c < d && nums[d] == nums[d - 1]) d --;
                        c ++;
                        d --; 
                    }else if ((long)nums[a] + nums[b] + nums[c] + nums[d] < target) {
                        c ++;
                    }else {
                        d --;
                    }
                }
            }
        }

        return res;
    }
};
```

总结一下2-4数之和

两数之和使用的方法是哈希，因为题目要求是返回满足条件的下标，所以不能进行排序，不能使用双指针算法

三数之和和四数都使用的是双指针算法，不同的是四数之和需要考虑target为负数的时候，这时候剪枝就需要满足大于等于0，然后就是四数之和可能会爆int，所以需要转化为long

还有一题四数求和，返回的是总数，用的是哈希计数
