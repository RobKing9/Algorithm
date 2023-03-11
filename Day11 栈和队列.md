# Day11 | 20. 有效的括号 | 1047. 删除字符串中的所有相邻重复项 | 150. 逆波兰表达式求值



## 20.[有效的括号](https://leetcode.cn/problems/valid-parentheses/description/)

```cpp
class Solution {
public:
    bool isValid(string s) {
        // 一定为偶数
        if(s.size() % 2 != 0) return false;
        stack<char> myStack;
        for(int i = 0; i < s.size(); i ++ ) {
            // 左边 入栈
            if(s[i] == '(' || s[i] == '[' || s[i] == '{') myStack.push(s[i]);
            else {
                // 开始为右边
                if(myStack.empty()) return false;
                // 三种情况
                char top = myStack.top();
                if( (s[i] == ')' && top == '(') || (s[i] == ']' && top == '[') || (s[i] == '}' && top == '{') )     					 myStack.pop();
                else return false;
            }
        }
        // 没有匹配完毕
        return myStack.empty();
    }
};
```

注意几个细节：

- 开始的偶数判断
- 左边先入的情况
- 不满足三种情况返回false
- 最后没有匹配完成的情况

## [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for(int i = 0; i < s.size(); i ++ ) {
            if(st.empty() || s[i] != st.top()) st.push(s[i]);
            else st.pop();
        }
        string res;
        while(!st.empty()) {
            res.push_back(st.top());
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};

```



## [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        // 思路：数字入栈，运算符取出栈顶两个数字运算，结果入栈，返回栈顶
        if(tokens.size() == 0) return -1;
        stack<string> st;

        for(int i = 0; i < tokens.size() ; i++ ) {
            string tmp = tokens[i];
            if(tmp == "+" || tmp == "-" || tmp == "*" || tmp == "/") {
                int a = stoi(st.top());
                st.pop();
                int b = stoi(st.top());
                st.pop();
                if(tmp == "+") st.push(to_string(a + b));
                if(tmp == "-") st.push(to_string(b - a));
                if(tmp == "*") st.push(to_string(b * a));
                if(tmp == "/") st.push(to_string(b / a));
            }else st.push(tmp);
        }
        return stoi(st.top());
    }
};
```

```golang
func evalRPN(tokens []string) int {
    arr := make([]int,0)
    var res int
    //遍历数组
    for _, v := range tokens {
        //如果是字符 将两个数pop出来进行计算
        if v=="+" || v=="-" || v=="*" || v=="/" {
            //pop两个数 怎么取出来？
            //栈顶
            b := arr[len(arr)-1]
            //栈顶前
            a := arr[len(arr)-2]
            res = handle(a, b, v)
            arr = arr[:len(arr)-2]
            arr = append(arr,res)
        } else {
            //是数字加入栈
            vv,_ := strconv.Atoi(v)
            arr = append(arr, vv)
        }
    }
    return arr[0]
}

func handle(x, y int, token string) int {
    if token=="+" {
        return x+y
    }
    if token=="-" {
        return x-y
    }
    if token=="*" {
        return x*y
    }
    if token=="/" {
        return x/y
    }
    return -1
}
```

