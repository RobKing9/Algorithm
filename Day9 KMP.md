# Day9 | KMP

## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串的第一个匹配项的下标（下标从 0 开始）。如果 needle 不是 haystack 的一部分，则返回  -1 。

```cpp
class Solution {
public:
    void get(int * next, const string &s) {
        int j = 0;
        next[0] = 0;    // 初始化，第一个为0
        // 循环构造next数组，0已经有值了，从1开始
        for(int i = 1; i < s.size(); i ++ ) {
            while(j > 0 && s[i] != s[j]) {
                // 最长公共前后缀不相等, 回到上一个位置对应的下标
                j = next[j - 1];
            }
            if(s[i] == s[j]) j ++; // 最长公共前后缀相等, j往后移动
            next[i] = j;    // 赋值 
        }

    }
    int strStr(string haystack, string needle) {
        int next[needle.size()];
        // 获取next数组
        get(next, needle);
        int j = 0;
        for(int i = 0; i < haystack.size(); i ++ ) {    // i指针用来遍历主串
            // 匹配失败，通过next数组找到跳到的下标
            while(j > 0 && needle[j] != haystack[i]) j = next[j - 1];    
            if(needle[j] == haystack[i]) j ++;  // 匹配成功
            // 所有的都匹配成功
            if(j == needle.size()) return (i - needle.size() + 1);
        }
        return -1;
    }
};
```

## [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

暴力

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int size = s.size();
        // 重复串 > 1, <= size / 2
        for(int i = 1; i <= size / 2; i ++ ) {
            // 一定是能被size整除
            if(size % i == 0) {
                // 匹配
                bool isMatch = true;
                for(int j = i; j < size; j ++ ) {
                    // 匹配失败
                    if(s[j] != s[j - i]) {
                        isMatch = false;
                        break;
                    }
                }
                if(isMatch) return true;
            }
        }
        return false;
    }
};
```

移动匹配

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        // 移动匹配
        string str = s + s;
        // 去掉头和尾依然可以找到原串
        str.erase(str.begin());
        str.erase(str.end() - 1);
        // std::string::npos表示不存在的位置，就是说在str中找到了s返回true
        if(str.find(s) != std::string::npos) return true;
        return false;
    }
};
```

KMP

```cPP
class Solution {
public:
    void getNext(int * next, const string & s) {
        int j = 0;
        next[0] = 0;
        for(int i = 1; i < s.size(); i ++ ) {
            while(j > 0 && s[i] != s[j]) j = next[j - 1];
            if(s[i] == s[j]) j ++;
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern(string s) {
        int size = s.size();
        int next[size];
        getNext(next, s);
        // next[size - 1]是最后一个元素的next数组下标
        // size - next[size - 1] 是
        // 说明数组的长度正好可以被 (数组长度-最长相等前后缀的长度) 整除 ，说明该字符串有重复的子字符串
        // 数组长度减去最长相同前后缀的长度相当于是第一个周期的长度，也就是一个周期的长度，如果这个周期可以被整除，就说明整个数组就是这个周期的循环。
        if(next[size - 1] != 0 && size % (size - (next[size - 1])) == 0) return true;
        return false;
    }
};
```

