# Ch10 Dynamic Programming

## Day 32

**理论基础**

dp数组，想清楚index和存放的value都表示什么意义就好做了

下标以及初始化要多加小心，有错就print dp数组



**509. Fibonacci**
```
def fib(self, n: int) -> int:
      if n == 0:
          return 0
      dp = [0] * (n+1)
      dp[1] = 1

      for i in range(2, n+1):
          dp[i] = dp[i-1] + dp[i-2]
      return dp[n]
```

**70.爬楼梯**

```
def climbStairs(self, n: int) -> int:
      if n == 1:
          return 1
      dp = [0] * (n+1)
      dp[1] = 1
      dp[2] = 2
      for i in range(3, n+1):
          dp[i] = dp[i-1] + dp[i-2]
      return dp[n]
```

**746.最小花费爬楼梯**
题目有点难懂但是并不难，和前面爬楼梯的思路很像
```
def minCostClimbingStairs(self, cost: List[int]) -> int:
      length = len(cost)
      dp = [float('inf')] * (length + 1)
      dp[0] = 0
      dp[1] = 0

      for i in range(2, len(dp)):
          min_val = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
          dp[i] = min_val
      return dp[length]
```
## Day 33

**62. 不同路径**

有趣起来了，有点小学奥数的感觉

如何初始化和建立2D的dp数组是看了一会视频题解才想出来的，自己把代码写出来了

```
def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for i in range(n)] for j in range(m)]
        for i in range(n):
            dp[0][i] = 1
        for j in range(m):
            dp[j][0] = 1
        for p in range(1, m):
            for q in range(1, n):
                dp[p][q] = dp[p-1][q] + dp[p][q-1]
        print(dp)
        return dp[m-1][n-1]
```

**63. 不同路径 II**

大体思路和上一题是一样的，但有个初始化的坑没有避开，如果障碍是在上边和左边的时候，在边上后面的元素都没法到达的，所以应该直接break

```
def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        num_row = len(obstacleGrid)
        num_col = len(obstacleGrid[0])
        dp = [[0 for i in range(num_col)] for j in range(num_row)]
        for a in range(num_row):
            if obstacleGrid[a][0] == 0:
                dp[a][0] = 1
            else: break #坑
        for b in range(num_col):
            if obstacleGrid[0][b] == 0:
                dp[0][b] = 1
            else: break #坑
        
        for p in range(1, num_row):
            for q in range(1, num_col):
                if obstacleGrid[p][q] == 1:
                    dp[p][q] = 0 #可以写成continue
                else:
                    dp[p][q] = dp[p-1][q] + dp[p][q-1]
        return dp[num_row-1][num_col-1]
```

**343. 整数拆分 积最大**

因为一个整数可以被拆分成2个，3个，4个。。。（10 拆成3*3*4 = 36最大）都有可能是最大的。我们要利用dp的list去避免重复运算

index表示数值和，value是拆分该数值的最大积

say i=10；j = 1,2,...9 固定的，i-j可以进行拆分/不拆分，比如说如果i-j = 2, dp[i-j] = 1

dp[i] = max(dp[i], j * dp[i-j], j * [i-j])

优化空间，j < i //2 + 1, 因为数字越接近，积最大

```
def integerBreak(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[2] = 1
        for i in range(n+1):
            for j in range(1, i//2 +1):
                dp[i] = max(dp[i], j *dp[i-j], j * (i-j))
        return dp[n]
```



**96. 不同的BST**

思路：左边子树的不同方式 * 右边子树的方式 = Total ways

```
def numTrees(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[0] = 1
        dp[1] = 1

        for i in range(2,n+1):
            print(f"i is {i}")
            for j in range(i):
                dp[i] += dp[j] * dp[i-1-j]
        return dp[n]
```

## 0-1 背包问题

2维dp数组：任取的这个概念理解起来有点抽象

初始化：dp[i][0] = 0; dp[0][weight[0]:] = value[0]

dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i])


1维dp数组：对我来讲比较容易些, nested loop的思路

dp[i] = max(dp[i], dp[i-weight[j]] + value[j]

找零问题
```

```



