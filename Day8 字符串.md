# Day8 | 344.反转字符串 I II | 剑指Offer 05.替换空格

# |151.翻转字符串里的单词 | 剑指Offer58-II.左旋转字符串



## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for(int i = 0, j = s.size() - 1; i < j; i ++, j --) {
            char tmp = s[i];
            s[i] = s[j];
            s[j] = tmp;
        }
        // reverse(s.begin(), s.end());
        return;
    }
};
```

## [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。

- 如果剩余字符少于 k 个，则将剩余字符全部反转。

- 如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        // 每一次移动2k步
        for(int i = 0; i < s.size(); i += (2 * k) ) {
            // 剩余的字符大于k，小于2k
            if(i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k);
            }else {
                // 剩余的字符小于k
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
};
```

## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for(int i = 0; i < s.size(); i ++ ) {
            if(s[i] == ' ') res+= "%20";
            else res.push_back(s[i]);
        }
        return res;
    }
};
```

## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 s ，请你反转字符串中 单词 的顺序。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

返回 单词 顺序颠倒且 单词 之间用单个空格连接的结果字符串。

注意：输入字符串 s中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

```cpp
class Solution {
public:
    // 思路：先去掉空格，留下单词之间的空格，然后翻转每个单词，最后整体翻转
    void removeSpace(string &s) {
        int j = 0;
        for(int i = 0; i < s.size(); i ++ ) {
            // 不为空格的时候
            if(s[i] != ' ') {
                // 手动加单词间的空格
                if(j > 0) s[j ++] = ' ';
                // 添加单词
                while(i < s.size() && s[i] != ' ') {
                    s[j ++] = s[i ++];  // 通过移动覆盖的方式 添加到原串
                }
            }
        }
        // 重置大小
        s.resize(j);
    }
    void reverse(string & s, int st, int ed) {
        for(int i = st, j = ed; i < j; i ++, j --) swap(s[i], s[j - 1]);
    }
    string reverseWords(string s) {
        removeSpace(s);
        // 翻转每个单词
        for(int i = 0; i < s.size(); i ++) {
            int j = i;  // 记录单词的初始位置
            while(i < s.size() && s[i] != ' ') i ++;
            reverse(s, j, i);
        }
        // 整体翻转
        reverse(s, 0, s.size());
        return s;
    }
};
```



## [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string res;
        for(int i = n;  i < s.size(); i ++) res.push_back(s[i]);
        for(int i = 0; i < n; i ++ ) res.push_back(s[i]);

        return res;
    }
};
```

不申请额外的内存

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        // 反转前n
        reverse(s.begin(), s.begin() + n);
        // 反转n到最后
        reverse(s.begin() + n, s.end());
        // 整体反转
        reverse(s.begin(), s.end());
        return s;
    }
};
```

