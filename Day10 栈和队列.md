# Day10 | 232.用栈实现队列 | 225. 用队列实现栈



## [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

```cpp
class MyQueue {
public:
    stack<int>  stackIn;
    stack<int> stackOut;

    MyQueue() {

    }
    
    void push(int x) {
        stackIn.push(x);
    }
    
    int pop() {
        // 输出栈为空的时候，导入输入栈的全部数据
        if(stackOut.empty()) {
            while(!stackIn.empty()) {
                stackOut.push(stackIn.top());
                stackIn.pop();
            }
        }
        int top = stackOut.top();
        stackOut.pop();
        return top;
    }
    
    int peek() {
        int res = this->pop();
        stackOut.push(res);
        return res;
    }
    
    bool empty() {
        return stackIn.empty() && stackOut.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

## [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

```cpp
class MyStack {
public: 
    queue<int> que;
    MyStack() {

    }
    
    void push(int x) {
        que.push(x);
    }
    
    int pop() {
        // 将除了队尾元素之外的元素重新入队
        int size = que.size() - 1;
        while(size -- ) {
            que.push(que.front());
            que.pop();
        }
        int res = que.front();
        que.pop();
        return res;
    }
    
    int top() {
        return que.back();
    }
    
    bool empty() {
        return que.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

