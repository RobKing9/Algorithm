# Day39 动态规划 不同路径

● 62.不同路径 

● 63. 不同路径 II



## [62. 不同路径](https://leetcode.cn/problems/unique-paths/description/)

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for(int i = 1; i <= m; i ++ ) {
            for(int j = 1; j <= n; j ++ ) {
                if(i == 1 || j == 1) dp[i][j] = 1;
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m][n];
    }
};
```

## [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/description/)

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        if(obstacleGrid[0][0] != 1) dp[0][0] = 1;
        for(int i = 1; i < m; i ++ ) {
            if(obstacleGrid[i][0] != 1) dp[i][0] = dp[i-1][0]; 
        }
        for(int i = 1; i < n; i ++ ) {
            if(obstacleGrid[0][i] != 1) dp[0][i] = dp[0][i-1];
        }
        for(int i = 1; i < m; i ++ ) {
            for(int j = 1; j < n; j ++ ) {
                if(obstacleGrid[i][j] != 1) dp[i][j] = dp[i][j-1] + dp[i-1][j];
                else dp[i][j] = 0;
            }
        }
        return dp[m-1][n-1];
    }
};
```

