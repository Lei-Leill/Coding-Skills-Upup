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

背包问题

找零问题
```

```



