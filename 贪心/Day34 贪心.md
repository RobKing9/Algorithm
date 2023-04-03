# Day34 贪心

● 860.柠檬水找零 

● 406.根据身高重建队列 

● 452. 用最少数量的箭引爆气球 

## [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/description/)

在柠檬水摊上，每一杯柠檬水的售价为 `5` 美元。顾客排队购买你的产品，（按账单 `bills` 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 `5` 美元、`10` 美元或 `20` 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 `5` 美元。

注意，一开始你手头没有任何零钱。

给你一个整数数组 `bills` ，其中 `bills[i]` 是第 `i` 位顾客付的账。如果你能给每位顾客正确找零，返回 `true` ，否则返回 `false` 。

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int cnt5 = 0, cnt10 = 0;
        for(int i = 0; i < bills.size(); i ++ ) {
            if(bills[i] == 5) cnt5 ++;
            else if(bills[i] == 10) {
                // 没有5
                if(cnt5 < 1) return false;
                cnt5 --;    // 有5
                cnt10 ++;
            }else {
                // 10没有，5没有超过3张
                if(cnt10 < 1 && cnt5 < 3) return false;
                else if(cnt10 < 1 && cnt5 >= 3) cnt5 -= 3;  // 10没有，5有超过3张

                // 10有超过1张，5没有超过1张
                if(cnt10 >= 1 && cnt5 < 1) return false;
                else if(cnt10 >= 1 && cnt5 >= 1) {
                    // 10有超过1张，5有超过1张
                    cnt10 --;
                    cnt5 --;
                }
            }
        }
        return true;
    }
};
```

所以局部最优：遇到账单20，优先消耗美元10，完成本次找零。全局最优：完成全部账单的找零。

局部最优可以推出全局最优，并找不出反例

## [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)

假设有打乱顺序的一群人站成一个队列，数组 `people` 表示队列中一些人的属性（不一定按顺序）。每个 `people[i] = [hi, ki]` 表示第 `i` 个人的身高为 `hi` ，前面 **正好** 有 `ki` 个身高大于或等于 `hi` 的人。

请你重新构造并返回输入数组 `people` 所表示的队列。返回的队列应该格式化为数组 `queue` ，其中 `queue[j] = [hj, kj]` 是队列中第 `j` 个人的属性（`queue[0]` 是排在队列前面的人）。

```cpp
class Solution {
public:
    // 首先按照第一个元素从大到小排序，如果第一个相同，按照第二个从小到大排序
    static bool cmp(vector<int>& a, vector<int>& b) {
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        // 首先按照第一个元素从大到小排序，如果第一个相同，按照第二个从小到大排序
        sort(people.begin(), people.end(), cmp);
        // 根据第二个值选择插入的位置
        // vector<vector<int>> res;
        // 使用链表优化
        list<vector<int>> res;
        for(int i = 0; i < people.size(); i ++ ) {
            // 根据第二维确定位置
            int pos = people[i][1];
            // 将people[i]（是一个一维向量） 插入到 偏移为pos位置上
            // res.insert(res.begin() + pos, people[i]);
            // auto it = res.begin();
            list<vector<int>>::iterator it = res.begin();
            while(pos --) it ++;
            // 指定位置插入
            res.insert(it, people[i]);
        }
        // return res;
        return vector<vector<int>>(res.begin(), res.end());
    }
};
```

按照身高排序之后，优先按身高高的people的k来插入，后序插入节点也不会影响前面已经插入的节点，最终按照k的规则完成了队列。	

**局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性**

**全局最优：最后都做完插入操作，整个队列满足题目队列属性**



## [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)

有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 `points` ，其中`points[i] = [xstart, xend]` 表示水平直径在 `xstart` 和 `xend`之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 **完全垂直** 地射出。在坐标 `x` 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `x``start`，`x``end`， 且满足  `xstart ≤ x ≤ x``end`，则该气球会被 **引爆** 。可以射出的弓箭的数量 **没有限制** 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 `points` ，*返回引爆所有气球所必须射出的 **最小** 弓箭数* 。

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        return a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        // 按照左边界排序
        sort(points.begin(), points.end(), cmp);
        // 至少需要一支箭
        int res = 1;
        for(int i = 1; i < points.size(); i ++ ) {
            // 判断是否相交
            if(points[i][0] > points[i-1][1]) {
                // 不想交 需要一支箭
                res ++;
            }else {
                // 更新左边界 两个的靠左边的那个
                points[i][1] = min(points[i][1], points[i-1][1]);
            }
        }
        return res;
    }
};
```

局部最优：当气球出现重叠，一起射，所用弓箭最少。全局最优：把所有气球射爆所用弓箭最少。

**如果气球重叠了，重叠气球中右边边界的最小值 之前的区间一定需要一个弓箭**。