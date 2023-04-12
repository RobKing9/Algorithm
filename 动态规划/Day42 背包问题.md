# Day42 背包问题

## [01背包问题](https://www.acwing.com/problem/content/2/)

1. 确定dp数组以及下标的含义

   `dp[i][j]` 表示从 0-i 个物品选择  放进容量为j的时候 的最大价值

2. 递归表达式

   根据当前物品选或不选来推导出 `dp[i][j]`

   - 如果不选的话：`dp[i][j]` = `dp[i - 1][j]` 不放进去，背包里面的物品就是 i-1, 容量也还是j
   - 如果选择的话：`dp[i][j]` = `dp[i - 1][j - weight[i]] + value[i]` 就相当于，容量变小了（因为装了当前的物品），但是整体价值增加了

   所以整体的最大价值就是取两则最大值 `dp[i][j]` = `max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])`

3. 初始化

   j为0的时候，也就是容量为0，此时 `dp[i][0] = 0` 不能放物品所以是0

   i为0的时候，就是选择第0个物品，看容量是否大于`weight[0]` 大于的话就是 `value[0]`

4. 遍历顺序

   可以先遍历 0-i 个物品 再 遍历 容量为 0-j 。此时的填充顺序是一行一行的填充

   也可以先遍历 容量为 0-j，再遍历 放0-i 个物品。此时填充的顺序是一列一列的填充

   结果相同是因为 当前的值 都是依赖 上方的值和左上方的值

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1010;
int weight[N], value[N];
int dp[N][N];   // dp[i][j] 表示从0-i个物品选择 放进容量为 j 的最大价值

int main () {
    int n, bag;
    cin >> n >> bag;
    for(int i = 0; i < n; i ++ ) cin >> weight[i] >> value[i];
    // 初始化dp
    // 容量为0
    for(int i = 0; i < n; i ++ ) dp[i][0] = 0;
    // 第0个物品
    for(int i = weight[0]; i <= bag; i ++ ) dp[0][i] = value[0];
    // 遍历
    // 遍历 1-n 个物品
    for(int i = 1; i < n; i ++ ) {
        // 容量从 0-bag
        for(int j = 0; j <= bag; j ++ ) {
            dp[i][j] = dp[i - 1][j];    // 不选
            if(j >= weight[i]) dp[i][j] = max(dp[i][j], dp[i - 1][j - weight[i]] + value[i]);   // 选
        }
    }
    cout << dp[n - 1][bag];
    return 0;
}
```



背包问题 优化 滚动数组

之所以可以利用滚动数组，是因为当前的值只依赖上一次的结果，而不依赖之前的结果。所以计算出当前的值，然后覆盖之前的，下一次只需要利用当前值就行了

1. 确定dp数组以及下标的含义

   `dp[j]` 表示从 容量为j的时候 的最大价值

2. 递归表达式

   根据当前物品选或不选来推导出 `dp[j]`

   - 如果不选的话：`dp[j]` = `dp[j]` 相当于 不覆盖原来的数据，直接拷贝原来的
   - 如果选择的话：`dp[j]` = `dp[j - weight[i]] + value[i]` 就相当于，容量变小了（因为装了当前的物品），但是整体价值增加了

   所以整体的最大价值就是取两则最大值 `dp[j]` = `max(dp[j], dp[j - weight[i]] + value[i])`

3. 初始化

   j为0的时候，也就是容量为0，此时 `dp[0] = 0` 不能放物品所以是0

4. 遍历顺序

   先遍历 0-i 个物品 

   背包的容量必须要从大到小遍历。如果从小到大的话 同一个物品 会被放进两次

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1010;
int weight[N], value[N];
int dp[N];

int main () {
    int n, bag;
    cin >> n >> bag;
    for(int i = 0; i < n; i ++ ) cin >> weight[i] >> value[i];
    
    // 初始化
    dp[0] = 0;
    for(int i = 0; i < n; i ++ ) {
        for(int j = bag; j >= weight[i]; j --) {
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bag] << endl;
    
    return 0;
}
```



## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int size = nums.size();
        int sum = 0;
        for(int i = 0; i < size; i ++) sum += nums[i];
        int tmp = sum / 2;
        if(sum%2) return false;
        vector<vector<int>> dp(size + 1, vector<int>(sum + 10, 0));
        dp[0][0] = dp[0][nums[0]] = 1;

        for(int i = 1; i < size; i ++ ) {
            for(int j = 0; j <= tmp; j ++) {
                if(j >= nums[i])    dp[i][j] = dp[i - 1][j - nums[i]]; // 选
                dp[i][j] |= dp[i - 1][j];
            }
        }
        return dp[size-1][tmp];
    }
};
```

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int size = nums.size();
        int sum = 0;
        for(int i = 0; i < size; i ++) sum += nums[i];
        int tmp = sum / 2;
        if(sum%2) return false;
        // vector<vector<int>> dp(size + 1, vector<int>(sum + 10, 0));
        vector<int> dp(sum + 10, 0);
        dp[0] = dp[nums[0]] = 1;

        for(int i = 1; i < size; i ++ ) {
            for(int j = tmp; j >= nums[i]; j --) {
                dp[j] |= dp[j - nums[i]];
                // if(j >= nums[i])    dp[i][j] = dp[i - 1][j - nums[i]]; // 选
                // dp[i][j] |= dp[i - 1][j];
            }
        }
        return dp[tmp];
    }
};
```

