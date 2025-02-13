# Day 1

数组： 内存空间地址是连续的

**704. Binary Search**

做过一次，一遍过

区间的界定是关键
  
左闭右开

```
  def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) #
        while left < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else: #>
                right = mid
        return -1
```
左闭右闭

```
        left = 0
        right = len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else: 
                right = mid - 1
        return -1
```

TODO 34， 35， 69， 367

**27. Remove Element**

1. 快慢指针，用类比，一个萝卜一个坑，
2. 快指针去探索决定，慢指针锁定位置
3. 判定基本都是依据快指针来判定
```
def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        fast = 0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
            #if nums[slow] != val: #这一行错误的原因是会重复使用不为val的元素，因为如果nums[slow]!=val，自动往后就有问题
                slow += 1
            fast += 1
        return slow
```

TODO: 26, 283, 844, 977

**97. 有序数组的平方**

重要思路点：两头的平方最大，因此从两头开始计算然后加入array

双指针，一个在前一个在后，有点像Binary Search的思路

```
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        length = len(nums)
        sqaure_list = [0 for i in range(length)]
        pointer1, pointer2, index = 0, length -1, length -1
        while pointer1 <= pointer2:
            sqr1 = nums[pointer1] ** 2
            sqr2 = nums[pointer2] ** 2
            if sqr1 > sqr2:
                sqaure_list[index] = sqr1
                pointer1 += 1
            else:
                sqaure_list[index] = sqr2
                pointer2 -= 1
            index -= 1
        return sqaure_list
```

# Day2

**209.长度最小的子数集**

题目是找大于等于，总是误会为等于

滑动窗口，之前做过，但没完全掌握

思考方向： 一开始，慢指针停留在原点，快指针一步步往后，若是sum大于等于了，先update min_length，尝试将慢指针往后，缩小窗口，直到sum小于target了，
如果快指针还没有到最右，则往右移动，探索别的可能性

```
def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        slow, fast, current_sum = 0, 0, 0, 0
        min_len = len(nums) + 1
        while fast < len(nums):
            current_sum += nums[fast]
            while current_sum >= target:
                current_sum -= nums[slow]
                min_len = min(min_len, fast - slow + 1)
                slow += 1
            fast += 1    
        return min_len if min_len != len(nums) + 1 else 0
```

第一遍做错误点，如何移动滑动窗口错
```
def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        slow, fast, current_sum, current_length = 0, 0, 0, 0
        min_len = len(nums)
        while fast < len(nums):
            current_sum += nums[fast]
            curr_length += 1
            if current_sum == target:
                min_len = min(curr_length, min_len)
                slow += 1
                fast += 1
            elif current_sum > target:
                current_sum -= nums[slow]
                slow += 1
            else:
                fast += 1
        return min_len
```

**59.螺旋矩阵II**

之前做过，debug花了一会， 大的思路还记得

1. 旋转次数 = n//2
2. 边界问题考虑清楚，range method [)
3. n为奇数，中间的点为最大的，需补上
```
def generateMatrix(self, n: int) -> List[List[int]]:
        output = [[0 for i in range(n)] for j in range(n)]
        iteration = n // 2
        m = 0
        num = 1
        mini, maxi= 0, n-1
        while m < iteration:
            for j in range(mini, maxi):
                output[mini][j] = num
                num += 1
            
            for i in range(mini, maxi):
                output[i][maxi] = num
                num += 1
            
            for j in range(maxi, mini, -1):
                output[maxi][j] = num
                num += 1
            
            for i in range(maxi, mini, -1):
                output[i][mini] = num
                num += 1
            m += 1
            mini += 1
            maxi -= 1
        if n % 2 == 1:
            output[iteration][iteration] = n**2

        return output
```


**58. 区间和**

https://kamacoder.com/problempage.php?pid=1070

前缀和：为了避免多次计算，我们一开始初始化一个list，装从第一个到现在元素的sum，所以比如求2-5之间的sum，我们只需要用sum list[5]- [1]即可解决问题

**44.开发商购买土地**

https://kamacoder.com/problempage.php?pid=1044

找横向and纵向的分割线，思路就是前缀和，算出差值的办法还挺有趣的： abs(sum - 2 * partial_sum)


# 总结
![image](https://github.com/user-attachments/assets/6048884b-541d-4f92-b6a0-6f7819ff50b6)

     
