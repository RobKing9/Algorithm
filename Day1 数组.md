# Day1 | 数组理论基础 | 704. 二分查找 | 27. 移除元素

## [704. 二分查找](https://leetcode.cn/problems/binary-search/)

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid] == target) {
                return mid;
            }else if(nums[mid] > target) {
                right = mid - 1;
            }else {
                left = mid + 1;
            }
        }
        return -1;
    }
};
```

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size();
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] == target) {
                return mid;
            }else if(target > nums[mid]) {
                left = mid + 1;
            }else {
                right = mid;
            }
        }
        return -1;
    }
};
```

理解：以区间的方式理解，[left, right]。这个时候是 while(left <= right)此时是有意义的。目标值 target < nums[mid] 在左侧 right=mid-1 变成区间[left, mid -1].

## [27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for(int i = 0; i < size; i ++) {
            if(nums[i] == val) {
                for(int j = i + 1; j < size; j ++ ) {
                    nums[j - 1] = nums[j];
                }
                i--;
                size--;
            }
        }
        return size;
    }
};
```

理解：向前移动一位，i--。用一个size来保存长度。

```C++
//双指针
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        int left = 0, right = size - 1;
        while(left <= right) {
            if(nums[right] == val) {
                size --;
                right --;
            }else {
                if(nums[left] == val) {
                    size --;
                    nums[left] = nums[right];
                    nums[right] = val;
                    right --;
                }
                left ++;
            }
        }
        return size;
    }
};
// 快慢指针
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for(int fast = 0; fast < nums.size(); fast ++ ) {
            if(nums[fast] != val) {
                nums[slow ++] = nums[fast];
            }
        }
        return slow;
    }
};
```



理解：用快指针的值覆盖慢指针的值，返回满指针的下标
