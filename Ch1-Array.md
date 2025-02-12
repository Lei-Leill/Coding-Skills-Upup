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

快慢指针，用类比，一个萝卜一个坑，快指针去探索决定
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
      
     
