# Day13 | 滑动窗口最大值 | 347.前 K 个高频元素



## [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/description/)

```cpp
class Solution {
public:
    // 思路：形成单调递减队列，这样每个窗口的最大值就是队头了
    // 队列用来存储下标，这样就可以根据队头元素下标来判断 队头是否已经不在窗口了
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> qu;
        for(int i = 0; i < nums.size(); i ++ ) {
            // 窗口中最小的下标 大于队头的下标，说明队头要滑出去了
            if(!qu.empty() && i - k + 1 > qu[0]) qu.pop_front();
            // 和队尾比较，如果大于，队尾出队
            while(!qu.empty() && nums[i] > nums[qu.back()]) qu.pop_back();
            // 入队
            qu.push_back(i);

            // 至少k个 形成窗口
            if(i + 1 >= k ) res.push_back(nums[qu[0]]);
        }
        return res;
    }
};
```

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> myMap;
        for(auto &num : nums) myMap[num] ++;

        // 按照map的value进行排序
        struct myComparison {
            bool operator()(pair<int, int> & p1, pair<int, int> & p2) {
                return p1.second > p2.second;   //小顶堆是大于号
            }
        };
        // 使用小根堆
        priority_queue<pair<int, int>, vector<pair<int, int>>, myComparison> qu;
        // 
        for(auto it = myMap.begin(); it != myMap.end(); it ++ ) {
            qu.push({it->first, it->second});
            // 维持队列的长度只有k，开始加入到队列中都是值小的，出队即可，留下的都是值大的
            if(qu.size() > k) qu.pop();
        }
        // for(auto &m : myMap) {
        //     qu.push(m);
        //     if(qu.size() > k) qu.pop();
        // }
        vector<int> res;
        while(!qu.empty()) {
            res.push_back(qu.top().first);
            qu.pop();
        }

        return res;

    }
};
```

