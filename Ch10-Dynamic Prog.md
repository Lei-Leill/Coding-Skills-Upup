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

**2维dp数组**

dp[i][j]表示：任取[0:i]的物品 装进j容量的背包，得到的最大价值

初始化：第一列为0；第一行大于等于物品0的重量的初始化为value[0]

递推公式dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i])


**1维dp数组**

通过滚动数组来优化空间，因为本质上我们只是通过上一行来推下一行，所以压缩为1d数组的时候，要考虑怎么在遍历的时候不覆盖上一行我们之后要用到的数值

通过2d数组我们可以观察到确定dp[i][j]取决于dp[i-1][j]和dp[i-1][j-weight[i]，上一行，正上边和左边的两个值

所以遍历顺序是先物品(2d里的行），再倒叙遍历背包容量，从右向左，这样左边和上面的值还没有被更新（污染），所以不会出现一个物品被使用多次的情况

dp[i] = max(dp[i], dp[i-weight[j]] + value[j]

**416. 分割等和子集**

因为每个数字也只可以被使用一次，所以可以看成是weight和value相等的0-1背包问题

因为weight和value相等，所以我们可以用算法去看sum//2的值是否被装满来看是否能被分割
```
def canPartition(self, nums: List[int]) -> bool:
        sum = 0
        for i in nums:
            sum += i
        if sum %2 == 1: return False
        target = sum // 2
        dp = [0] * (target+1)
        
        for i in nums:
            for j in range(target, i-1, -1):
                dp[j] = max(dp[j], dp[j-i] + i)

        if (dp[target] == target):
            return True
        return False
```
**1049. 石头撞击，最后一块石头的重量**

有点被题目这句话“每次任取两块石头进行撞击” 给绕进去而去考虑微观上的每次撞击

其实需要将题目转化为宏观上 如何将一堆石头分成差尽量小的两组，最后剩下的重量即为：sum-dp[target]*2

**494. 目标和**

对于dp，还是不是很有思路，对于怎么样能套进0-1背包的模版想不到啊

因为符号有加有减，所以我们可以考虑数组分成两组，一组是加的和，一组是减的和

**474. 一和零：二进制数字字符串最长子串**

要找m和n的最长

代码reference问题：
```
dp = [[0] * (n+1)] * (m+1)
# all rows in dp point to the same sub-list in memory.
# Changing one row changes them all, since they’re really just different references to the same object.
```

**Immutable Objects** (integers, floats, strings, tuples, booleans)

Once created, you cannot change the content or value of these objects in place. If you “change” them, you’re really creating a new object.

```
a = 10
b = a
# Both a and b point to the same integer object 10
a = 20
# a now points to a new integer object 20; b still points to 10
You can’t update a to 20 “in place” because 10 (an int) is immutable. Python just makes a point to a new integer object 20.
```

**Mutable Objects**（lists, dictionaries, sets, most class-based objects）

You can change the contents or attributes of these objects in place.

```
my_list = [1, 2, 3]
another_list = my_list
my_list.append(4)
# both variables see the change
print(my_list)       # [1, 2, 3, 4]
print(another_list)  # [1, 2, 3, 4]
Here, both my_list and another_list refer to the same list object, so changing the list through one variable is visible through the other.
```

## 完全背包问题

### 组合与排列 -> 遍历顺序的差异
**518. 零钱兑换的方式**
coins = [1,2,5], target = 5, return 4
1. 一共是1,1,1,1,1 ;  1,1,1,2 ;  1,2,2 ; 5   四种情况
2. 之所以先物品再背包，就是保证放入背包时物品顺序是恒定的，即coins[1]一定在coins[0]之后 (e.g. 只可能出现1，1，1，2 不会有1，2，1，1 etc），coins[2]一定在coins[1]后面

**377.组合总和**

和上一题不一样，顺序在这里是重要的，1，1，1，2 和 1，2，1，1是两种组合

先背包再物品

**322.零钱兑换的最小纸币数**

dp[j] = min(dp[j], dp[j-coins[i]] + 1

用类似backtracking的思路，往前一步推，要到达当前j的金额，上一步只可能是从j-coins[i]来，从那里来多加一枚纸币，

我们要找这几种可能性所能达到纸币数量最少的情况，所以初始化的时候要设为infinity，但是dp[0] = 0

**279.完全平方数**

和上一题很像
```
def numSquares(self, n: int) -> int:
        num = int(sqrt(n))
        nums = [i*i for i in range(1,num+1)]

        dp = [float('inf')] * (n+1)
        dp[0] = 0
        for i in nums:
            for j in range(n+1):
                if j >= i:
                    dp[j] = min(dp[j], dp[j-i] + 1)
        return dp[n]
```
